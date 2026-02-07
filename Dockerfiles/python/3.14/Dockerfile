ARG PYTHON_VERSION=3.14.3
ARG PREFIX=/opt/python/${PYTHON_VERSION}

FROM ghcr.io/omedosef/debian:13 AS builder

LABEL maintainer="OmedoSef"
LABEL org.opencontainers.image.source=https://github.com/OmedoSef/DevContainers
LABEL org.opencontainers.image.licenses=Apache-2.0
LABEL org.opencontainers.image.description="A Debian 13-based development container with zsh"

ARG PYTHON_VERSION
ARG PREFIX

ENV DEBIAN_FRONTEND=noninteractive

USER root
# Toolchain + headers for optional stdlib modules
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    ca-certificates \
    curl \
    xz-utils \
    pkg-config \
    # core + common optional modules
    zlib1g-dev \
    libssl-dev \
    libbz2-dev \
    libffi-dev \
    liblzma-dev \
    libreadline-dev \
    libsqlite3-dev \
    libncursesw5-dev \
    libgdbm-dev \
    libgdbm-compat-dev \
    tk-dev \
    uuid-dev \
    libzstd-dev \
  && rm -rf /var/lib/apt/lists/*


WORKDIR /usr/src

RUN --mount=type=cache,target=/var/cache/downloads \
    set -euo pipefail; \
    url="https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz"; \
    f="/var/cache/downloads/Python-${PYTHON_VERSION}.tgz"; \
    curl -fL --retry 5 --retry-delay 2 -o "${f}.tmp" "${url}"; \
    mv "${f}.tmp" "${f}"; \
    tar -xzf "${f}"

WORKDIR /usr/src/Python-${PYTHON_VERSION}

# Configure + build + install
# - --enable-optimizations: enables PGO (slower build). Remove if you want faster image build.
# - --with-ensurepip=install: ships pip.
RUN ./configure --prefix="${PREFIX}" --with-ensurepip=install \
 && make -j"$(nproc)" \
 && make altinstall


############################
# 2) Runtime
############################
FROM ghcr.io/omedosef/debian:13
ARG PYTHON_VERSION
ARG PREFIX


# Copy compiled Python
COPY --from=builder "${PREFIX}" "${PREFIX}"
ENV PATH="${PREFIX}/bin:${PATH}"

RUN sudo ln -s "${PREFIX}/bin/python${PYTHON_VERSION%.*}" /usr/local/bin/python \
  && sudo ln -s "${PREFIX}/bin/python${PYTHON_VERSION%.*}" /usr/local/bin/python3 \
  && sudo ln -s "${PREFIX}/bin/pip${PYTHON_VERSION%.*}" /usr/local/bin/pip

