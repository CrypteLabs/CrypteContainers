FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl gzip procps tar wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/alertmanager-0.24.0-152-linux-amd64-debian-11.tar.gz && \
    echo "0f0878b5b060e568e0a0a943ff473d3cf7cae593a4ee8935ccaf21097bfee7c9  /tmp/bitnami/pkg/cache/alertmanager-0.24.0-152-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/alertmanager-0.24.0-152-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/alertmanager-0.24.0-152-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN ln -sf /opt/bitnami/alertmanager/conf /etc/alertmanager
RUN ln -sf /opt/bitnami/alertmanager/data /alertmanager
RUN chmod g+rwX /opt/bitnami

RUN mkdir -p /opt/bitnami/alertmanager/data/ && chmod g+rwX /opt/bitnami/alertmanager/data/
ENV APP_VERSION="0.24.0" \
    BITNAMI_APP_NAME="alertmanager" \
    PATH="/opt/bitnami/alertmanager/bin:$PATH"

EXPOSE 9093

WORKDIR /opt/bitnami/alertmanager
USER 1001
ENTRYPOINT [ "/opt/bitnami/alertmanager/bin/alertmanager" ]
CMD [ "--config.file=/opt/bitnami/alertmanager/conf/config.yml", "--storage.path=/opt/bitnami/alertmanager/data" ]
