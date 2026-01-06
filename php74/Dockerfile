FROM php:7.4-fpm

# Sistem paketlerini güncelle ve gerekli paketleri kur
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libzip-dev \
    libicu-dev \
    libpq-dev \
    libonig-dev \
    git \
    unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
    gd \
    mysqli \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    mbstring \
    zip \
    intl \
    opcache \
    exif \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Not: MongoDB PHP Extension PHP 8.1+ gerektirir, PHP 7.4 için desteklenmiyor

# Composer kurulumu
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Çalışma dizinini ayarla
WORKDIR /var/www/html

# PHP-FPM portunu açık tut
EXPOSE 9000

CMD ["php-fpm"]

