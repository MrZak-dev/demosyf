FROM php:8.4-fpm-bookworm

ARG TIMEZONE

COPY docker/php/php.ini /usr/local/etc/php/conf.d/docker-php-config.ini

RUN apt-get update && apt-get install -y \
    gnupg \
    g++ \
    procps \
    openssl \
    git \
    unzip \
    zlib1g-dev \
    libzip-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg-dev \
    libicu-dev  \
    libonig-dev \
    libxslt1-dev \
    amqp-tools \
    librabbitmq-dev \
    librabbitmq4 \
    libpq-dev \
    acl 

RUN curl -sL https://deb.nodesource.com/setup_20.x | bash
#install yarn
RUN apt-get install -y nodejs  

RUN npm install -g yarn

RUN curl -sL https://get.symfony.com/cli/installer | bash


RUN docker-php-ext-configure gd --with-jpeg --with-freetype 

RUN docker-php-ext-install \
    pdo pdo_mysql  pdo_pgsql zip xsl gd intl opcache exif mbstring  bcmath

# Extract PHP source
RUN docker-php-source extract

RUN docker-php-source delete
# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
    && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
    && "date"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/symfony
