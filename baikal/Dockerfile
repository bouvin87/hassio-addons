FROM ghcr.io/home-assistant/base:latest

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV VERSION=0.12.1

# Setup base
RUN \
    apk add --no-cache \
        nginx \
        php82 \
        php82-curl \
        php82-fpm \
        php82-mbstring \
        php82-xml \
        php82-sqlite3 \
        php82-pdo \
        php82-pdo_mysql \
        php82-pdo_sqlite \
        php82-session \
        php82-dom \
        php82-xmlreader \
        php82-xmlwriter \
        curl \
        unzip \
    \
    && curl -Ls \
        "https://github.com/sabre-io/Baikal/releases/download/$VERSION/baikal-$VERSION.zip" \
        -o "baikal-$VERSION.zip" \
    \
    && unzip -q "baikal-$VERSION.zip" -d /var/www/ \
    \
    && rm "baikal-$VERSION.zip" \
    \
    && rm -rf \
        /tmp/* \
        /etc/nginx

# Copy root filesystem
COPY rootfs /

# Correct permissions for s6 v3
RUN \
    if [ -d /etc/cont-init.d ]; then \
        chmod -R 755 /etc/cont-init.d; \
    fi \
    && if [ -d /etc/services.d ]; then \
        chmod -R 755 /etc/services.d; \
    fi

WORKDIR "/data"

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version="${BUILD_VERSION}" \
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created="${BUILD_DATE}" \
    org.opencontainers.image.revision="${BUILD_REF}" \
    org.opencontainers.image.version="${BUILD_VERSION}"
