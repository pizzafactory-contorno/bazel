FROM pizzafactory0contorno/piatto:20.04-ubuntu@sha256:990771a6adc07dfd8a444633a4b07c39c01a48b40f6017e0eb4d626d9bd7cb31 as artifact

ARG BAZELISK_VERSION
ARG TARGETPLATFORM

USER root
RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y \
      apt-transport-https curl gnupg zip unzip git \
      openjdk-11-jdk-headless python3 python3-pip python3-distutils \
 && curl https://storage.googleapis.com/git-repo-downloads/repo | sed -e 's|env python|env python3|' > /usr/bin/repo \
 && chmod a+x /usr/bin/repo \
 && case ${TARGETPLATFORM} in \
         "linux/amd64")  TARGETARCH=amd64  ;; \
         "linux/arm64")  TARGETARCH=arm64  ;; \
    esac \
 && curl -sL https://github.com/bazelbuild/bazelisk/releases/download/v${BAZELISK_VERSION}/bazelisk-linux-${TARGETARCH} > /usr/bin/bazel \
 && chmod a+x /usr/bin/bazel \
 && DEBIAN_FRONTEND=noninteractive apt-get full-upgrade -y \
 && apt-get clean && rm -rf /var/lib/apt/lists/*
USER user

FROM artifact as check

RUN /usr/bin/bazel
