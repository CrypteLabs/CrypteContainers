FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl gzip libc6 procps tar wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/airflow-exporter-0.20220314.0-151-linux-amd64-debian-11.tar.gz && \
    echo "94cd4d4e13ce0807fb126c7ca8f83ab6729cd954a3049033906dcaece0d3e227  /tmp/bitnami/pkg/cache/airflow-exporter-0.20220314.0-151-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/airflow-exporter-0.20220314.0-151-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/airflow-exporter-0.20220314.0-151-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami

ENV APP_VERSION="0.20220314.0" \
    BITNAMI_APP_NAME="airflow-exporter" \
    PATH="/opt/bitnami/airflow-exporter/bin:$PATH"

EXPOSE 9112

WORKDIR /opt/bitnami/airflow-exporter
USER 1001
ENTRYPOINT [ "airflow-prometheus-exporter" ]
