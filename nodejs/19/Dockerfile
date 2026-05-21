FROM        --platform=$TARGETOS/$TARGETARCH node:19-bullseye-slim

LABEL       author="Embotic" maintainer="luke@embotic.xyz"

# Fix QEMU ARM64 issues by using --no-install-recommends and avoiding problematic packages
RUN         apt update \
            && apt install -y --no-install-recommends \
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
                tini \
            && apt-get clean \
            && rm -rf /var/lib/apt/lists/* \
            && useradd -m -d /home/container container \
            && rm -rf /usr/bin/dd \
            && rm -rf /usr/bin/fallocate \
            && rm -rf /usr/bin/truncate \
            && rm -rf /usr/bin/xfs_mkfile

RUN         npm install npm@9.8.1 typescript ts-node @types/node --location=global

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
