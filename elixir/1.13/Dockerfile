FROM --platform=$TARGETOS/$TARGETARCH elixir:1.13.4-slim

LABEL author="MrAvox" maintainer="mravox@proton.me"

RUN  apt update \
            && apt -y install git dnsutils curl iproute2 ffmpeg tini \
            && useradd -m -d /home/container container \
            && rm -rf /usr/bin/dd \
            && rm -rf /usr/bin/fallocate \
            && rm -rf /usr/bin/truncate \
            && rm -rf /usr/bin/xfs_mkfile

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
