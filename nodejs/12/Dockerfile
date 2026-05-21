FROM        --platform=$TARGETOS/$TARGETARCH node:12-bullseye-slim

LABEL       author="Embotic" maintainer="luke@embotic.xyz"

# Set apt to retry on transient errors
RUN         echo 'Apt::Retries "3";' > /etc/apt/apt.conf.d/99-retries && \
            echo 'Apt::Get::AllowUnauthenticated "true";' > /etc/apt/apt.conf.d/99-allow-unauthenticated

RUN         apt update --allow-unauthenticated \
            && apt -y install --no-install-recommends \
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
            && rm -rf /usr/bin/dd \
            && rm -rf /usr/bin/fallocate \
            && rm -rf /usr/bin/truncate \
            && rm -rf /usr/bin/xfs_mkfile \
            && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN         npm install npm@9.8.1 typescript ts-node @types/node --location=global

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
