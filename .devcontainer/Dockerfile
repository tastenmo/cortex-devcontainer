FROM martinescad/clang-arm-cortex:latest

USER root

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        binutils-multiarch \
        ca-certificates \
        gdb-multiarch \
        libfreetype6 \
        libgtk-3-0 \
        libsm6 \
        libusb-1.0-0 \
        libxext6 \
        libxinerama1 \
        libxi6 \
        libxrandr2 \
        libxrender1 \
        libxtst6 \
        udev \
        xz-utils \
    && rm -rf /var/lib/apt/lists/*

COPY .devcontainer/vendor/ /tmp/jlink/

RUN set -eux; \
    installer="$(find /tmp/jlink -maxdepth 1 -type f \( -name 'JLink_Linux_*.deb' -o -name 'JLink_Linux_*.tgz' \) | sort | tail -n 1)"; \
    if [ -z "$installer" ]; then \
        echo "Missing SEGGER J-Link installer. Put JLink_Linux_*.deb or JLink_Linux_*.tgz in .devcontainer/vendor/." >&2; \
        exit 1; \
    fi; \
    case "$installer" in \
        *.deb) \
            apt-get update; \
            # J-Link postinst reloads udev rules, which fails during image build.
            # Temporarily stub udevadm so package configuration can complete.
            dpkg-divert --local --rename --add /usr/bin/udevadm; \
            printf '%s\n' '#!/bin/sh' 'exit 0' > /usr/bin/udevadm; \
            chmod +x /usr/bin/udevadm; \
            apt-get install -y --no-install-recommends "$installer"; \
            rm -f /usr/bin/udevadm; \
            dpkg-divert --rename --remove /usr/bin/udevadm; \
            rm -rf /var/lib/apt/lists/*; \
            ;; \
        *.tgz) \
            tar -xzf "$installer" -C /opt; \
            jlink_dir="$(find /opt -maxdepth 1 -type d -name 'JLink_Linux_*' | sort | tail -n 1)"; \
            [ -n "$jlink_dir" ]; \
            ln -sf "$jlink_dir/JLinkExe" /usr/local/bin/JLinkExe; \
            ln -sf "$jlink_dir/JLinkGDBServerCLExe" /usr/local/bin/JLinkGDBServerCLExe; \
            ln -sf "$jlink_dir/JLinkRTTClient" /usr/local/bin/JLinkRTTClient; \
            ln -sf "$jlink_dir/JFlashLiteExe" /usr/local/bin/JFlashLiteExe; \
            ;; \
    esac; \
    rm -rf /tmp/jlink

USER vscode