ARG VARIANT=buster
FROM mcr.microsoft.com/vscode/devcontainers/base:${VARIANT}

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install make gcc-arm-none-eabi

WORKDIR /root
RUN wget https://github.com/microsoft/uf2/raw/master/utils/uf2conv.py
