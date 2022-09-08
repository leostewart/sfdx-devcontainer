# [Choice] Node.js version (use -bullseye variants on local arm64/Apple Silicon): 18-bullseye, 16-bullseye, 14-bullseye, 18-buster, 16-buster, 14-buster
ARG VARIANT=16-bullseye
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:${VARIANT}

# https://github.com/salesforcecli/sfdx-cli/blob/main/dockerfiles/Dockerfile_full
ENV DEBIAN_FRONTEND=noninteractive
ARG SALESFORCE_CLI_VERSION=latest-rc
ARG SF_CLI_VERSION=latest-rc
RUN npm install --global sfdx-cli@${SALESFORCE_CLI_VERSION} --ignore-scripts
RUN npm install --global @salesforce/cli@${SF_CLI_VERSION}
RUN apt-get update && apt-get install --assume-yes openjdk-11-jdk-headless jq
RUN apt-get autoremove --assume-yes \
  && apt-get clean --assume-yes \
  && rm -rf /var/lib/apt/lists/*
ENV SFDX_CONTAINER_MODE true
ENV DEBIAN_FRONTEND=dialog
ENV SHELL /bin/bash

ARG USERNAME=node

# https://code.visualstudio.com/remote/advancedcontainers/persist-bash-history
RUN SNIPPET="export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" \
    && mkdir /commandhistory \
    && touch /commandhistory/.bash_history \
    && chown -R $USERNAME /commandhistory \
    && echo "$SNIPPET" >> "/home/$USERNAME/.bashrc"

RUN mkdir /home/node/.sf && chown -R $USERNAME /home/node/.sf
RUN mkdir /home/node/.sfdx && chown -R $USERNAME /home/node/.sfdx

USER $USERNAME
