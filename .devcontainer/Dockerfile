FROM mcr.microsoft.com/vscode/devcontainers/python:3.10
USER vscode
ENV HOME=/home/vscode
ENV WORKSPACE=$HOME/repositories/nbsp1221/customized-auto-gpt
WORKDIR $WORKSPACE
RUN sudo chown -R vscode:vscode $HOME

# Remove ImageMagick due to https://security-tracker.debian.org/tracker/CVE-2019-10131
RUN sudo apt update && \
    sudo apt purge -y imagemagick imagemagick-6-common

# Upgrade Python packages due to https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2022-40897
RUN python3 -m pip install --upgrade setuptools

# Install GitHub CLI
RUN curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null && \
    sudo apt update && \
    sudo apt install -y gh

# Install Zsh
RUN cd $HOME/repositories/nbsp1221 && \
    git clone https://github.com/nbsp1221/dotfiles.git && \
    cd dotfiles && \
    ./zsh/installer.sh

# Install Chrome for web browsing
RUN sudo apt update && \
    curl -sSL https://dl.google.com/linux/direct/google-chrome-stable_current_$(dpkg --print-architecture).deb -o /tmp/chrome.deb && \
    sudo apt install -y /tmp/chrome.deb

# Install Node.js
RUN sudo rm -rf $NVM_DIR && \
    export NVM_DIR="" && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash && \
    /bin/zsh -c "source ~/.zshrc && nvm install --lts && nvm use --lts && corepack enable"
