FROM php:7.3-cli-alpine

ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

RUN chmod uga+x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions \
    zip \
    intl \
    calendar \
    curl

RUN apk update

RUN apk add \
    nodejs \
    yarn \
    openssh-client \
    git \
    bash \
    rsync \
    nano \
    composer

RUN composer global require hirak/prestissimo

RUN yarn global add cross-env eslint

