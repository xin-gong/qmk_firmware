FROM ubuntu:22.04

ENV LANG C.UTF-8

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Install runtime dependencies and essential development tools
RUN set -eux; \
    apt-get update; \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        apt-utils \
        git-all \
        git-lfs \
        python3 \
        python3-pip \
        sudo \
        usbutils \
        vim-gtk3 \
    ; \
    groupadd --gid $USER_GID $USERNAME; \
    useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME; \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Install other dependencies via QMK scripts
RUN --mount=target=/opt/qmk_firmware,rw=true \
    /opt/qmk_firmware/util/qmk_install.sh; \
    mkdir -p /etc/udev/rules.d; \
    cp /opt/qmk_firmware/util/udev/50-qmk.rules /etc/udev/rules.d

# Switch to non root user then install QMK python package
USER $USERNAME
RUN python3 -m pip install qmk
