FROM ghcr.io/omedosef/debian:13 AS builder

LABEL maintainer="OmedoSef"
LABEL org.opencontainers.image.source=https://github.com/OmedoSef/DevContainers
LABEL org.opencontainers.image.licenses=Apache-2.0
LABEL org.opencontainers.image.description="A Debian 13-based with Rust development container with zsh"


ENV RUST_VERSION=1.94


RUN sudo apt update && sudo apt install -y build-essential \
    && curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -q -y --default-toolchain ${RUST_VERSION}