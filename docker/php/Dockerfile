# Ubuntu base PHP
FROM php:7.4-fpm

# Deps.
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libzip-dev \
    libicu-dev \
    libxml2-dev \
    libonig-dev \
    unzip \
    bash \
    pkg-config \
    htop \
    mc \
    libmemcached-dev \
    libsasl2-dev \
    autoconf \
    g++ \
    make \
    libtool \
    wget \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd zip mysqli opcache pdo_mysql intl soap bcmath mbstring \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && rm -rf /var/lib/apt/lists/* /tmp/pear

# IonCube Loader
RUN wget https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz -O /tmp/ioncube_loaders.tar.gz \
    && tar -zxvf /tmp/ioncube_loaders.tar.gz -C /tmp \
    && PHP_EXT_DIR=$(php -i | grep ^extension_dir | awk '{print $3}') \
    && cp /tmp/ioncube/ioncube_loader_lin_7.4.so $PHP_EXT_DIR \
    && echo "zend_extension=$PHP_EXT_DIR/ioncube_loader_lin_7.4.so" > /usr/local/etc/php/conf.d/00-ioncube-loader.ini \
    && rm -rf /tmp/ioncube_loaders.tar.gz /tmp/ioncube

# Set permission
RUN chown -R www-data:www-data /var/www/html

# Copy PHP setting
COPY ../../config/php/php.ini /usr/local/etc/php/
