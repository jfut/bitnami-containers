FROM bitnami/minideb:bullseye as builder

ARG TARGETARCH

ENV OS_ARCH="${TARGETARCH:-amd64}"

COPY prebuildfs /

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install required system packages and dependencies
RUN install_packages ca-certificates curl gzip tar
RUN mkdir -p /tmp/bitnami/pkg/cache/ && cd /tmp/bitnami/pkg/cache/ && \
    if [ ! -f rmq-default-credential-updater-1.0.2-153-linux-${OS_ARCH}-debian-11.tar.gz ]; then \
      curl -SsLf https://downloads.bitnami.com/files/stacksmith/rmq-default-credential-updater-1.0.2-153-linux-${OS_ARCH}-debian-11.tar.gz -O ; \
      curl -SsLf https://downloads.bitnami.com/files/stacksmith/rmq-default-credential-updater-1.0.2-153-linux-${OS_ARCH}-debian-11.tar.gz.sha256 -O ; \
    fi && \
    sha256sum -c rmq-default-credential-updater-1.0.2-153-linux-${OS_ARCH}-debian-11.tar.gz.sha256 && \
    tar -zxf rmq-default-credential-updater-1.0.2-153-linux-${OS_ARCH}-debian-11.tar.gz -C /opt/bitnami --strip-components=2 --no-same-owner --wildcards '*/files' && \
    rm -rf rmq-default-credential-updater-1.0.2-153-linux-${OS_ARCH}-debian-11.tar.gz

######

FROM scratch

ARG TARGETARCH

ENV OS_ARCH="${TARGETARCH:-amd64}"

LABEL org.opencontainers.image.authors="https://bitnami.com/contact" \
      org.opencontainers.image.description="Application packaged by Bitnami" \
      org.opencontainers.image.ref.name="1.0.2-scratch-r9" \
      org.opencontainers.image.source="https://github.com/bitnami/containers/tree/main/bitnami/rmq-default-credential-updater" \
      org.opencontainers.image.title="rmq-default-credential-updater" \
      org.opencontainers.image.vendor="VMware, Inc." \
      org.opencontainers.image.version="1.0.2"

COPY prebuildfs /
COPY rootfs /
COPY --from=builder /opt/bitnami/rmq-default-credential-updater/bin/default-user-credential-updater /default-user-credential-updater

USER 1001

ENTRYPOINT [ "/default-user-credential-updater" ]
