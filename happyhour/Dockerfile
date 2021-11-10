ARG BUILD_FROM=hassioaddons/base:10.2.2
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Copy root filesystem
COPY rootfs/etc /etc/
COPY \
    rootfs/opt/happyhour/package.json \
    rootfs/opt/happyhour/package-lock.json \
    /opt/happyhour/

# Setup base
# hadolint ignore=DL3003
RUN \
    apk add --no-cache --virtual .build-dependencies \
        npm=7.17.0-r0 \
    \
    && apk add --no-cache \
        nodejs=14.18.1-r0 \
    \
    && cd /opt/happyhour \
    \
    && npm install --production \
    \
    && npx mod-pruner prune --force \
    \
    && apk del --no-cache --purge .build-dependencies

COPY rootfs/opt/happyhour/public /opt/happyhour/public/
COPY rootfs/opt/happyhour/index.js /opt/happyhour/

# Build arugments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="HappyHour" \
    io.hass.description="A HappyHour add-on for Home Assistant" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Jorim Tielemans <tielemans.jorim@gmail.com>" \
    org.label-schema.description="A HappyHour add-on for Home Assistant" \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="HappyHour" \
    org.label-schema.schema-version="1.0" \
    org.label-schema.url="https://addons.community" \
    org.label-schema.usage="https://gitlab.com/filii-lamberti/addon-happyhour" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://gitlab.com/filii-lamberti/filii-addons" \
    org.label-schema.vendor="Filii Lamberti"
