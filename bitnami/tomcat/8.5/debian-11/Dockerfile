FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

ARG JAVA_EXTRA_SECURITY_DIR="/bitnami/java/extra-security"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libc6 libssl1.1 procps tar xmlstarlet zlib1g
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "java" "11.0.15-1-1" --checksum 6d176a8b3c894c608106fee2cf10aaf3771015defb0b7e08fe60ce6c9c1cd342
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "tomcat" "8.5.81-6" --checksum 91e8a8845fd3d3b661c9221273e3da644cfc8724ccbe16a26d1e226df72e9da1
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.3-151" --checksum 9690a34674f152e55c71a55275265314ed1bb29e0be8a75d7880488509f70deb
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-152" --checksum 0c751c7e2ec0bc900a19dbec0306d6294fe744ddfb0fa64197ba1a36040092f0
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/java/postunpack.sh
RUN /opt/bitnami/scripts/tomcat/postunpack.sh
ENV APP_VERSION="8.5.81" \
    BITNAMI_APP_NAME="tomcat" \
    JAVA_HOME="/opt/bitnami/java" \
    PATH="/opt/bitnami/java/bin:/opt/bitnami/tomcat/bin:/opt/bitnami/common/bin:$PATH"

EXPOSE 8009 8080

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/tomcat/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/tomcat/run.sh" ]
