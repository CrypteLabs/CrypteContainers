FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl gzip procps tar wget
RUN wget -nc -P /tmp/bitnami/pkg/cache/ https://downloads.bitnami.com/files/stacksmith/contour-1.20.2-1-linux-amd64-debian-11.tar.gz && \
    echo "cbf9e700cf81efb2351a3020e616d234665b01dcf9f643f4c7ee9ad912d47c04  /tmp/bitnami/pkg/cache/contour-1.20.2-1-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf /tmp/bitnami/pkg/cache/contour-1.20.2-1-linux-amd64-debian-11.tar.gz -P --transform 's|^[^/]*/files|/opt/bitnami|' --wildcards '*/files' && \
    rm -rf /tmp/bitnami/pkg/cache/contour-1.20.2-1-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN ln -s /opt/bitnami/contour/bin/contour /bin/contour

ENV APP_VERSION="1.20.2" \
    BITNAMI_APP_NAME="contour" \
    PATH="/opt/bitnami/contour/bin:$PATH"

USER 1001
ENTRYPOINT [ "contour" ]
