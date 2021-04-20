ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base/amd64:4.1.2
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Setup base
# hadolint ignore=DL3003
RUN \
    apt-get update \
    && apt-get install -y \
	make \
        openssl \
        python \
        autoconf \
        automake \
        libtool \
        libusb-dev \
        libneon27-dev \
        snmpd \
        libsnmp-dev \
        git \        
    && git clone https://github.com/networkupstools/nut.git /tmp/nut \
    && cd /tmp/nut \
    #&& git checkout ever_hid_ups \
    && ./autogen.sh \
    && ./configure \
            --disable-dependency-tracking \
            --prefix=/usr \
            --libexecdir=/usr/lib/nut \
            --without-wrap \
            --with-user=root \
            --with-group=root \
            --disable-static \
            --with-serial \
            --with-usb \
            --without-avahi \
            --with-snmp \
            --with-neon \
            --without-powerman \
            --without-ipmi \
            --without-freeipmi \
            --with-libltdl \
            --without-cgi \
            --with-drvpath=/usr/lib/nut \
            --datadir=/usr/share/nut \
            --sysconfdir=/etc/nut \
            --with-statepath=/var/run/nut \
            --with-altpidpath=/var/run/nut \
    && make install \
    && rm -f -r \
        /tmp/* \
        /etc/nut/*.apk-new

# Copy root filesystem
COPY rootfs /

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
    io.hass.version=${BUILD_VERSION} \
    maintainer="Dale Higgs <dale3h@addons.community>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Dale Higgs <dale3h@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
