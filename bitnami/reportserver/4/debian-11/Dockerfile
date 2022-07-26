FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

ARG JAVA_EXTRA_SECURITY_DIR="/bitnami/java/extra-security"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libaudit1 libc6 libcap-ng0 libgcc-s1 libicu67 liblzma5 libncurses6 libpam0g libssl1.1 libstdc++6 libtinfo6 libxml2 procps tar xmlstarlet zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "java" "11.0.15-1-1" --checksum 6d176a8b3c894c608106fee2cf10aaf3771015defb0b7e08fe60ce6c9c1cd342
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "tomcat" "9.0.65-0" --checksum cc5ec8f674abfb18c745f3656356aa83344bc0d9ff24024bffd3f85ef485514c
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "mysql-client" "10.6.8-151" --checksum c85e4be9bcee70c86c7bc7e13742e2d97810ad8f7d6154f8b66811b6cc4d0948
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "reportserver" "4.2.0-6066-1" --checksum 7f39e172f23e10759b3c4a97da78fd3f2fd2a2f2c146c396004f363462b1ceb8
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.3-151" --checksum 9690a34674f152e55c71a55275265314ed1bb29e0be8a75d7880488509f70deb
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-152" --checksum 0c751c7e2ec0bc900a19dbec0306d6294fe744ddfb0fa64197ba1a36040092f0
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/java/postunpack.sh
RUN /opt/bitnami/scripts/mysql-client/postunpack.sh
RUN /opt/bitnami/scripts/reportserver/postunpack.sh
RUN /opt/bitnami/scripts/tomcat/postunpack.sh
ENV APP_VERSION="4.2.0-6066" \
    BITNAMI_APP_NAME="reportserver" \
    JAVA_HOME="/opt/bitnami/java" \
    PATH="/opt/bitnami/java/bin:/opt/bitnami/tomcat/bin:/opt/bitnami/mysql/bin:/opt/bitnami/common/bin:$PATH" \
    TOMCAT_START_RETRIES="60"

EXPOSE 8009 8080 8443

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/reportserver/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/tomcat/run.sh" ]
