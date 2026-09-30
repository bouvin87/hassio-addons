FROM ghcr.io/home-assistant/base:latest

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV VERSION=0.12.1

RUN apk update \
    && apk add --no-cache \
        nginx \
        curl \
        unzip \
        php82 \
        php82-curl \
        php82-fpm \
        php82-mbstring \
        php82-pdo \
        php82-pdo_mysql \
        php82-pdo_sqlite \
        php82-dom \
        php82-simplexml \
        php82-openssl \
        php82-fileinfo \
        php82-iconv \
    && curl -Ls \
        "https://github.com/sabre-io/Baikal/releases/download/${VERSION}/baikal-${VERSION}.zip" \
        -o "/tmp/baikal-${VERSION}.zip" \
    && unzip -q "/tmp/baikal-${VERSION}.zip" -d /var/www/ \
    && rm "/tmp/baikal-${VERSION}.zip" \
    && rm -rf /etc/nginx

COPY rootfs /

RUN \
    if [ -d /etc/cont-init.d ]; then \
        chmod -R 755 /etc/cont-init.d; \
    fi \
    && if [ -d /etc/services.d ]; then \
        chmod -R 755 /etc/services.d; \
    fi

WORKDIR "/data"

ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

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
