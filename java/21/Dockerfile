FROM --platform=$TARGETOS/$TARGETARCH node:22-bookworm-slim

LABEL author="Embotic" maintainer="luke@embotic.xyz"

RUN apt update \
    && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini \
    && useradd -m -d /home/container container \
    && rm -rf /usr/bin/dd /usr/bin/fallocate /usr/bin/truncate /usr/bin/xfs_mkfile

RUN npm install -g npm@latest typescript ts-node @types/node

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
