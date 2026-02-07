FROM debian:13

LABEL maintainer="OmedoSef"
LABEL org.opencontainers.image.source=https://github.com/OmedoSef/DevContainers
LABEL org.opencontainers.image.licenses=Apache-2.0
LABEL org.opencontainers.image.description="A Debian 13-based development container with zsh"

ARG USER_NAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y zsh zplug curl neovim sudo 

# Create the user vscode with sudo access
RUN groupadd --gid $USER_GID $USER_NAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USER_NAME -s /bin/zsh \
    && echo $USER_NAME ALL=\(ALL\) NOPASSWD:ALL > /etc/sudoers.d/$USER_NAME \
    && chmod 0440 /etc/sudoers.d/$USER_NAME



# Set USER to vscode
USER $USER_NAME

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

COPY .zshrc /home/$USER_NAME/.zshrc
