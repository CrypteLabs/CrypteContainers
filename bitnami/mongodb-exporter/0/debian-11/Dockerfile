FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl gzip procps tar wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/mongodb-exporter-0.33.0-1-linux-amd64-debian-11.tar.gz && \
    echo "a741ad3f14c1943c423235c5fadcf6aa0260120429c0b8671d87fac6554123f1  /tmp/bitnami/pkg/cache/mongodb-exporter-0.33.0-1-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/mongodb-exporter-0.33.0-1-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/mongodb-exporter-0.33.0-1-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN ln -sf /opt/bitnami/mongodb-exporter/bin/mongodb_exporter /bin/mongodb_exporter

ENV APP_VERSION="0.33.0" \
    BITNAMI_APP_NAME="mongodb-exporter" \
    PATH="/opt/bitnami/mongodb-exporter/bin:$PATH"

EXPOSE 9216

WORKDIR /opt/bitnami/mongodb-exporter
USER 1001
ENTRYPOINT [ "mongodb_exporter" ]
