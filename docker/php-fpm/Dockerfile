ARG VERSION

# Dev image
FROM php:7.3-fpm-alpine AS dev

## Install system dependencies
RUN apk update && \
    apk add --update linux-headers && \
    apk add --update curl && \
    apk add --no-cache --virtual dev-deps git autoconf gcc g++ make && \
    apk add --no-cache zlib-dev libzip-dev

## Install php extensions
RUN pecl install xdebug-2.9.0 && \
    docker-php-ext-enable xdebug && \
    docker-php-ext-install pdo_mysql zip
## Copy php default configuration
COPY docker/php-fpm/default.ini /usr/local/etc/php/conf.d/default.ini

ENV APP_ENV=dev
WORKDIR /app

## Install composer
RUN curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php &&\
    HASH=`curl -sS https://composer.github.io/installer.sig` && \
    php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer 
    
# Test image
FROM dev AS test
RUN composer update
WORKDIR /app

## Copy project files to workdir
COPY . .

## Install application dependencies
RUN composer install --no-interaction --optimize-autoloader

## Change files owner to php-fpm default user
RUN chown -R www-data:www-data .

# Prod image
FROM test AS prod

ENV APP_ENV=prod
WORKDIR /app

## Remove dev dependencies
RUN composer install --no-dev --no-interaction --optimize-autoloader

## Disable xdebug on production
RUN rm /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

## Cleanup
RUN apk del dev-deps && \
    composer global remove hirak/prestissimo && \
    rm /usr/local/bin/composer
