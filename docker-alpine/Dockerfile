FROM php:8.1-cli-alpine3.15

ARG NODE_VERSION=16.16.0
ARG YARN_VERSION=1.22.19

RUN echo "@main http://dl-cdn.alpinelinux.org/alpine/v3.15/main" >> /etc/apk/repositories \
 && echo "@community http://dl-cdn.alpinelinux.org/alpine/v3.15/community" >> /etc/apk/repositories \
 && apk update

RUN apk add --no-cache tar bind-tools curl ca-certificates libmemcached \
    zip unzip git supervisor sqlite libcap libpng-dev python2 \
 && docker-php-ext-configure gd \
 && pecl install swoole \
 && pecl install pcov \
 && pecl install msgpack \
 && pecl install redis \
 && pecl install igbinary \
 && pecl install xdebug \
 && docker-php-ext-enable xdebug swoole pcov msgpack redis igbinary \
 && docker-php-ext-install mysql ldap intl zip bcmath soap imap \
 && pecl install memcached \
 && docker-php-ext-enable memcached \
 && php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer

COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN curl -fsSLO --compressed "https://unofficial-builds.nodejs.org/download/release/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64-musl.tar.xz" \
 && tar -xJf "node-v$NODE_VERSION-linux-x64-musl.tar.xz" -C /usr/local --strip-components=1 --no-same-owner \
 && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
 && rm -f "node-v$NODE_VERSION-linux-x64-musl.tar.xz"

RUN curl -fsSLO --compressed "https://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz" \
 && mkdir -p /opt \
 && tar -xzf yarn-v$YARN_VERSION.tar.gz -C /opt/ \
 && ln -s /opt/yarn-v$YARN_VERSION/bin/yarn /usr/local/bin/yarn \
 && ln -s /opt/yarn-v$YARN_VERSION/bin/yarnpkg /usr/local/bin/yarnpkg \
 && rm -f yarn-v$YARN_VERSION.tar.gz

ENTRYPOINT []
CMD []
