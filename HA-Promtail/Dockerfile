ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base/amd64:5.2.3
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base system
ARG BUILD_ARCH=amd64

RUN \
# Promtail Version
    promtail="v2.4.2" \
    \
    && ARCH="${BUILD_ARCH}" \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; fi \
    && if [ "${BUILD_ARCH}" = "armv7" ]; then ARCH="armhf"; fi \
# Get Promtail Bin fiel
    && curl -O -L /tmp/promtail.zip \
        "https://github.com/grafana/loki/releases/download/{promtail}/loki-linux-{ARCH}.zip" \
# extract the binary
    && unzip promtail.zip \
# make sure it is executable
    && chmod a+x ["/promtail-linux-{ARCH}"] \
# move it
    && mv ["/promtail-linux-{ARCH}"] ["/usr/bin/promtail"] \
# cleanup download
    && rm -fr \
        /tmp/*

# Copy root filesystem
COPY rootfs /

ENTRYPOINT ["/usr/bin/promtail"]

CMD ["-config.file=/etc/promtail/config.yml"]

