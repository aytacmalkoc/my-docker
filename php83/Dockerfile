FROM php:8.3-fpm

# Sistem paketlerini güncelle ve gerekli paketleri kur
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libzip-dev \
    libicu-dev \
    libpq-dev \
    libonig-dev \
    libssl-dev \
    libcurl4-openssl-dev \
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
    bcmath \
    pcntl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# MongoDB PHP Extension kurulumu
RUN pecl install mongodb && docker-php-ext-enable mongodb

# Composer kurulumu
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Laravel için gerekli extension'lar zaten yukarıda kuruldu
# (pdo_mysql, mbstring, zip, intl, bcmath)

# Çalışma dizinini ayarla
WORKDIR /var/www/html

# PHP-FPM portunu açık tut
EXPOSE 9000

CMD ["php-fpm"]

