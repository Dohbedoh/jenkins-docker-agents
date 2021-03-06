FROM ubuntu:17.10
MAINTAINER Allan Burdajewicz <allan.burdajewicz@gmail.com>

RUN groupadd -g 1000 jenkins && \
  useradd -m -u 1000 -g 1000 jenkins

RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y \
    openjdk-8-jre-headless \
    curl \
    git \
    gpg \
    bzip2 \
    xz-utils \
    make

ARG NODE_VERSION=9.8.0
RUN ARCH= && dpkgArch="$(dpkg --print-architecture)" \
  && case "${dpkgArch##*-}" in \
    amd64) ARCH='x64';; \
    ppc64el) ARCH='ppc64le';; \
    s390x) ARCH='s390x';; \
    arm64) ARCH='arm64';; \
    armhf) ARCH='armv7l';; \
    *) echo "unsupported architecture"; exit 1 ;; \
  esac \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-$ARCH.tar.xz" \
  && tar xJf "node-v$NODE_VERSION-linux-$ARCH.tar.xz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-$ARCH.tar.xz" \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
  && chmod +x /usr/local/bin/node \
  && npm install npm@5.7.0 -g

USER jenkins
WORKDIR /home/jenkins