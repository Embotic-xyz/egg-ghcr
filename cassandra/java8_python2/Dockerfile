FROM        --platform=$TARGETOS/$TARGETARCH adoptopenjdk/openjdk8:alpine-jre

LABEL       author="Embotic" maintainer="luke@embotic.xyz"

LABEL       org.opencontainers.image.source="https://github.com/Embotic-xyz/embotic-xyz"
LABEL       org.opencontainers.image.licenses=MIT

RUN         apk add --update --no-cache python2 ca-certificates curl fontconfig git openssl sqlite tar tzdata \
				&& python -m ensurepip --upgrade && pip install pytz \
			    && adduser -D -h /home/container container \
				&& rm -rf /usr/bin/dd \
				&& rm -rf /usr/bin/fallocate \
				&& rm -rf /usr/bin/truncate \
				&& rm -rf /usr/bin/xfs_mkfile

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh
CMD         [ "/bin/ash", "/entrypoint.sh" ]
