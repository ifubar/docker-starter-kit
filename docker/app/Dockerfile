FROM php:7.0-fpm

# Install packages
RUN echo "deb http://ppa.launchpad.net/nginx/stable/ubuntu trusty main" > /etc/apt/sources.list.d/nginx-stable-trusty.list \
    && echo "deb-src http://ppa.launchpad.net/nginx/stable/ubuntu trusty main" >> /etc/apt/sources.list.d/nginx-stable-trusty.list \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys C300EE8C \
    && apt-get update \
    && apt-get install -y nginx supervisor

RUN docker-php-source extract \
    && buildDeps="libpq-dev libzip-dev libfreetype6-dev " \
    && apt-get install -y $buildDeps --no-install-recommends \
    && docker-php-ext-install pdo zip bcmath

# Setup config files
ADD docker/app/nginx.conf /etc/nginx/sites-enabled/default
ADD docker/app/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

WORKDIR "/app"

COPY ./app /app/

# Allow Composer to be run as root
ENV COMPOSER_ALLOW_SUPERUSER 1

# Setup the Composer installer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer

#uncomment
#RUN composer install  --no-interaction --no-dev --optimize-autoloader

RUN usermod -u 1001 www-data
RUN chown -R www-data .

EXPOSE 80

CMD ["/usr/bin/supervisord"]