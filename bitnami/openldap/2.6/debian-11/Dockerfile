FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libc6 libcom-err2 libcrypt1 libgssapi-krb5-2 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libltdl7 libnsl2 libnss3-tools libodbc1 libperl5.32 libsasl2-2 libssl1.1 libtirpc3 libwiredtiger0 libwrap0 mdbtools procps psmisc tar
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "openldap" "2.6.3-1" --checksum 1b185ccb560ebea6db28a31c3acb2ff19c78a493b88ffd55bd3f8cabcc438ca0
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-152" --checksum 0c751c7e2ec0bc900a19dbec0306d6294fe744ddfb0fa64197ba1a36040092f0
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/openldap/postunpack.sh
ENV APP_VERSION="2.6.3" \
    BITNAMI_APP_NAME="openldap" \
    PATH="/opt/bitnami/openldap/bin:/opt/bitnami/openldap/sbin:/opt/bitnami/common/bin:$PATH"

EXPOSE 1389 1636

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/openldap/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/openldap/run.sh" ]
