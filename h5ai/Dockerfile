FROM alpine:3.4
MAINTAINER Barra <bxt@mondedie.fr>

ENV H5AI_VERSION 0.29.0

RUN echo "@commuedge http://nl.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories \
 && echo "@testing http://nl.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
 && apk -U add \
    nginx \
    supervisor \
    tini@commuedge \
    php7-fpm@commuedge \
    php7-curl@commuedge \
    php7-iconv@commuedge \
    php7-xml@commuedge \
    php7-dom@commuedge \
    php7-openssl@commuedge \
    php7-json@commuedge \
    php7-zlib@commuedge \
    php7-pdo_mysql@commuedge \
    php7-pdo_sqlite@commuedge \
    php7-sqlite3@commuedge \
    php7-session@commuedge \
    wget \
    unzip \
    patch \
  && mkdir -p /var/www \
  && wget "http://release.larsjung.de/h5ai/h5ai-$H5AI_VERSION.zip" \
  && mkdir -p /usr/share/h5ai \
  && unzip "h5ai-$H5AI_VERSION.zip" -d /usr/share/h5ai
COPY class-setup.php.patch class-setup.php.patch
RUN patch -p1 -u -d /usr/share/h5ai/_h5ai/private/php/core/ -i /class-setup.php.patch && rm class-setup.php.patch

COPY nginx.conf /etc/nginx/nginx.conf
COPY supervisord.conf /etc/supervisor/supervisord.conf
COPY startup /usr/local/bin/startup

RUN chmod +x /usr/local/bin/startup
EXPOSE 80
VOLUME [ "/var/www" ]
CMD ["/sbin/tini","--","startup"]
