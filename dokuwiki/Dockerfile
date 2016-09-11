FROM alpine:latest
MAINTAINER Barra <bxt@mondedie.fr>

ENV DOKUWIKI_VERSION 2016-06-26a
ENV GID=1001 UID=1001

RUN echo "@commuedge http://nl.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories \
 && echo "@testing http://nl.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
 && apk -U add \
    nginx \
    supervisor \
    tini@commuedge \
    php7-fpm@testing \
    php7-curl@testing \
    php7-iconv@testing \
    php7-xml@testing \
    php7-dom@testing \
    php7-openssl@testing \
    php7-json@testing \
    php7-zlib@testing \
    php7-pdo_mysql@testing \
    php7-pdo_sqlite@testing \
    php7-sqlite3@testing \
    php7-session@testing \
    tar \
    curl \
  && mkdir -p /var/www \
  && cd /var/www \
  && curl -O -L "http://download.dokuwiki.org/src/dokuwiki/dokuwiki-$DOKUWIKI_VERSION.tgz" \
  && tar xzf "dokuwiki-${DOKUWIKI_VERSION}.tgz" --strip 1 \
  && rm "dokuwiki-$DOKUWIKI_VERSION.tgz"

COPY nginx.conf /etc/nginx/nginx.conf
COPY php-fpm.conf /etc/php7/php-fpm.conf
COPY supervisord.conf /etc/supervisor/supervisord.conf
COPY startup /usr/local/bin/startup

RUN chmod +x /usr/local/bin/startup
EXPOSE 80
VOLUME [ \
    "/var/www/data/pages", \
    "/var/www/data/meta", \
    "/var/www/data/media", \
    "/var/www/data/media_attic", \
    "/var/www/data/media_meta", \
    "/var/www/data/attic", \
    "/var/www/conf", \
    "/var/log" \
]
CMD ["/sbin/tini","--","startup"]
