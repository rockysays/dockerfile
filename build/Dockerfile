FROM debian:stretch

LABEL maintainer="mritd <mritd1234@gmail.com>"

ARG TZ='Asia/Shanghai'

ENV TZ ${TZ}

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV NVM_VERSION 0.33.11
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 8.11.3
ENV OPENJDK_VERSION 8.0.172-zulu
ENV ORACLEJDK_VERSION 8.0.171-oracle
ENV NPM_CACHE_DIR /data/npm_cache
ENV MAVEN_CACHE_DIR /data/maven_cache
ENV GRADLE_CACHE_DIR /data/gradle_cache
ENV GRADLE_USER_HOME ${GRADLE_CACHE_DIR}
ENV SDKMAN_INSTALL_URL https://get.sdkman.io
ENV NVM_INSTALL_URL https://raw.githubusercontent.com/creationix/nvm/v${NVM_VERSION}/install.sh

SHELL ["/bin/bash", "-c"]

RUN apt-get update -y \
    && apt-get upgrade -y \
    && apt-get install curl wget tar unzip zip bzip2 \
        tzdata git make gcc g++ libpng-dev locales -y \
    && locale-gen --purge en_US.UTF-8 zh_CN.UTF-8 \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 \
    && echo -e 'LANG="en_US.UTF-8"' > /etc/default/locale \
    && echo -e 'LANGUAGE="en_US:en"' >> /etc/default/locale \
    && mkdir -p ${NVM_DIR} ${NPM_CACHE_DIR} \
        ${MAVEN_CACHE_DIR} ${GRADLE_CACHE_DIR} \
    && curl -s ${NVM_INSTALL_URL} | bash \
    && curl -s ${SDKMAN_INSTALL_URL} | bash \
    && source ~/.bashrc \
    && sdk install maven \
    && sdk install gradle \
    && sdk install java ${ORACLEJDK_VERSION} \
    && sdk install java ${OPENJDK_VERSION} \
    && npm install -g npm yarn node-gyp gulp \
    && npm install -g --unsafe-perm build-tools \
    && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && rm -rf ~/.npm /var/lib/apt/lists/* /tmp/*

COPY sdkconfig /root/.sdkman/etc/config
COPY cnpm /usr/local/bin/cnpm
COPY cmvn /usr/local/bin/cmvn

VOLUME ${NPM_CACHE_DIR}
VOLUME ${MAVEN_CACHE_DIR}
VOLUME ${GRADLE_CACHE_DIR}

CMD ["bash"]
