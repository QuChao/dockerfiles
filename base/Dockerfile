FROM alpine
LABEL maintainer='Chao QU <mail@quchao.com>'

RUN set -ex; \
    # change mirrors
    sed -i 's|http://dl-cdn.alpinelinux.org|https://mirrors.aliyun.com|g' /etc/apk/repositories; \
    # update the repo file & force upgrade the pkgs
    apk upgrade --update-cache --available; \
    # common deps
    apk add --no-cache --virtual .common-deps \
        ca-certificates \
        su-exec \
        curl \
    ; \
    # reset CA list
    update-ca-certificates --fresh; \
    # cleanup
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/*;

ARG RUN_AS_USER
ENV RUN_AS_USER="${RUN_AS_USER:-nutshell}"

RUN set -ex; \
    # a low-privilege user
    addgroup -S "${RUN_AS_USER}"; \
    adduser -S -D -H  -h /var/empty -s /sbin/nologin -G "${RUN_AS_USER}" "${RUN_AS_USER}";

COPY docker-entrypoint.sh entrypoint-utils.sh /usr/local/bin/

RUN set -ex; \
    # test flight
	chmod +x /usr/local/bin/docker-entrypoint.sh;

RUN set -ex; \
    # avoid docker's 'Text file busy' issue
	docker-entrypoint.sh /bin/true;
