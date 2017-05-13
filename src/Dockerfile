FROM nginx:alpine

LABEL \
    maintainer="Morgan Auchede <morgan.auchede@gmail.com>"

ENV \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=2 \
    S6_KEEP_ENV=1 \
    WEBUI_ARIA2_VERSION=latest

RUN set -eux \

    # Install packages

    && apk add --no-cache --no-progress --virtual BUILD_DEPS \
           curl \
           git \

    # Install s6-overlay

    && export $(curl "https://raw.githubusercontent.com/timonier/version-lister/release/generated/s6-overlay/latest" | xargs) \

    && curl --location "https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz" | tar --directory / --extract --gzip \

    # Install syslog-stdout

    && export $(curl "https://raw.githubusercontent.com/timonier/version-lister/release/generated/syslog-stdout/latest" | xargs) \

    && curl --location "https://github.com/timonier/syslog-stdout/releases/download/v${SYSLOG_STDOUT_VERSION}/syslog-stdout.tar.gz" | tar --directory /usr/sbin --extract --gzip \

    # Install webui-aria2

    && git clone https://github.com/ziahamza/webui-aria2 /usr/share/webui-aria2 \

    # Clean

    && apk del --no-progress \
           BUILD_DEPS \

    && rm -f -r \
           /tmp/* \
           /usr/share/webui-aria2/.git

COPY ./rootfs /

ENTRYPOINT [ "/init" ]
