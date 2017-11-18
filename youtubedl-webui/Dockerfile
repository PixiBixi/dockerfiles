FROM alpine:latest
MAINTAINER Barra <bxt@mondedie.fr>

ENV GID=1001 UID=1001
ENV COMPOSER_VERSION=1.5.2

RUN echo "@commuedge http://nl.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories \
 && echo "@testing http://nl.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
 && apk -U add \
    nginx \
    ffmpeg \
    supervisor \
    tini@commuedge \
    php7@commuedge \
    php7-fpm@commuedge \
    php7-curl@commuedge \
    php7-iconv@commuedge \
    php7-xml@commuedge \
    php7-dom@commuedge \
    php7-json@commuedge \
    php7-zlib@commuedge \
    php7-session@commuedge \
    tar \
    git \
    curl \
  && mkdir -p /var/www \
  && cd /var/www \
  && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION} && chmod +x /usr/local/bin/composer \
  && curl -L https://yt-dl.org/downloads/latest/youtube-dl -o /usr/local/bin/youtube-dl && chmod a+x /usr/local/bin/youtube-dl \
  && git clone "https://github.com/PixiBixi/Youtube-dl-WebUI.git" youtube-dl && cd youtube-dl && composer dump-autoload \
  && rm -rf /var/cache/apk/* /tmp/*

COPY nginx.conf /etc/nginx/nginx.conf
COPY php-fpm.conf /etc/php7/php-fpm.conf
COPY supervisord.conf /etc/supervisor/supervisord.conf
COPY startup /usr/local/bin/startup

RUN chmod +x /usr/local/bin/startup
EXPOSE 80
VOLUME /var/www/youtube-dl/downloads

CMD ["/sbin/tini","--","startup"]

