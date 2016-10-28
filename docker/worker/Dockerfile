FROM php:7.0-cli

RUN apt-get update \
    && docker-php-source extract \
    && buildDeps="libpq-dev libzip-dev libfreetype6-dev " \
    && apt-get install -y $buildDeps --no-install-recommends \
    && docker-php-ext-install pdo zip bcmath

WORKDIR "/worker"

COPY ./worker /worker/

# Allow Composer to be run as root
ENV COMPOSER_ALLOW_SUPERUSER 1

# Setup the Composer installer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

#uncomment
#RUN composer install  --no-interaction --no-dev --optimize-autoloader

RUN usermod -u 1001 www-data
RUN chown -R www-data .

CMD ["php", "/worker/index.php"]
