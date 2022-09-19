FROM php:8.1-fpm-buster

COPY ./configs/php.ini /usr/local/etc/php/conf.d/docker-php-config.ini

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
    acl \
    && echo 'alias sf="php bin/console"' >> ~/.bashrc

RUN pecl install xdebug && docker-php-ext-enable xdebug
COPY ./configs/40-xdebug.ini /usr/local/etc/php/conf.d/

RUN docker-php-ext-configure gd --with-jpeg --with-freetype

RUN docker-php-ext-install \
    pdo pdo_mysql zip xsl gd intl opcache exif mbstring

# Set timezone
RUN ln -snf /usr/share/zoneinfo/Europe/Vilnius /etc/localtime && echo Europe/Vilnius > /etc/timezone \
    && printf '[PHP]\ndate.timezone = "%s"\n', Europe/Vilnius > /usr/local/etc/php/conf.d/tzone.ini \
    && "date"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN curl -1sLf https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh | bash
RUN apt install symfony-cli

WORKDIR /var/www/symfony_docker

EXPOSE 9000

CMD ["php-fpm"]