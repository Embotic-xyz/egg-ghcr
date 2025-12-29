FROM --platform=$TARGETOS/$TARGETARCH python:2.7-slim

LABEL author="Embotic" maintainer="luke@embotic.xyz"

RUN sed -i 's|deb.debian.org|archive.debian.org|g' /etc/apt/sources.list \
 && sed -i 's|security.debian.org|archive.debian.org|g' /etc/apt/sources.list \
 && echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99no-check-valid

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    ffmpeg \
    iproute2 \
    git \
    sqlite3 \
    libsqlite3-dev \
    python3 \
    python3-dev \
    ca-certificates \
    dnsutils \
    tzdata \
    zip \
    tar \
    curl \
    build-essential \
    libtool \
    iputils-ping \
    libnss3 \
    tini \
 && useradd -m -d /home/container container \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Harden without breaking dpkg (safe under buildx)
RUN chmod 000 /usr/bin/dd \
              /usr/bin/fallocate \
              /usr/bin/truncate \
              /usr/bin/xfs_mkfile || true

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
