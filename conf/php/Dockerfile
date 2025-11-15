FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
    cron \
    git \
    unzip \
    libicu-dev \
    libxml2-dev \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libxslt1-dev \
    libonig-dev \
    libcurl4-openssl-dev \
    libssl-dev \
    libmagickwand-dev \
    pkg-config \
    && apt-get clean

RUN docker-php-ext-configure gd \
    --with-jpeg \
    --with-freetype

RUN docker-php-ext-install \
    intl \
    pdo_mysql \
    soap \
    bcmath \
    zip \
    xsl \
    gd \
    mbstring \
    opcache \
    sockets \
    curl

RUN pecl install redis && docker-php-ext-enable redis
RUN pecl install imagick && docker-php-ext-enable imagick

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY php.ini /usr/local/etc/php/conf.d/custom.ini

WORKDIR /var/www/html

CMD ["php-fpm"]
