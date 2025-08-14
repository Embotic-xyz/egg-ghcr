FROM --platform=$TARGETOS/$TARGETARCH node:21-bookworm-slim

LABEL author="Embotic" maintainer="luke@embotic.xyz"

# Install system dependencies and create container user
RUN apt update \
    && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini \
    && useradd -m -d /home/container container \
    && rm -rf /usr/bin/dd /usr/bin/fallocate /usr/bin/truncate /usr/bin/xfs_mkfile

# Install global Node.js tools without upgrading npm
RUN npm install -g typescript ts-node @types/node

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

# Copy entrypoint script
COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]