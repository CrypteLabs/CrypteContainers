FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl gzip libc6 procps tar wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/cert-manager-webhook-1.9.1-0-linux-amd64-debian-11.tar.gz && \
    echo "d4d1d6cca151edfceebc8577b3380befe8f7781afa8920cbdb07f5f01ae0d7d1  /tmp/bitnami/pkg/cache/cert-manager-webhook-1.9.1-0-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/cert-manager-webhook-1.9.1-0-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/cert-manager-webhook-1.9.1-0-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami

ENV APP_VERSION="1.9.1" \
    BITNAMI_APP_NAME="cert-manager-webhook" \
    PATH="/opt/bitnami/cert-manager-webhook/bin:$PATH"

WORKDIR /opt/bitnami/cert-manager-webhook
USER 1001
ENTRYPOINT [ "/opt/bitnami/cert-manager-webhook/bin/cert-manager-webhook" ]
