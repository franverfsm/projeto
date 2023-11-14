FROM php:8.2-fpm

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential \
        libz-dev \
        libpq-dev \
        libjpeg-dev \
        libpng-dev \
        libssl-dev \
        libzip-dev \
        unzip \
        zip \
        locales \
        libjpeg62-turbo-dev \
        libfreetype6-dev \
        jpegoptim optipng pngquant gifsicle \
        vim \
        libzip-dev \
        unzip \
        libonig-dev \
        curl \
    && apt-get clean \
    && pecl install redis \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-configure zip \
    && docker-php-ext-install \
        gd \
        exif \
        opcache \
        pdo_mysql \
        mbstring \
        pcntl \
    && docker-php-ext-enable redis \
    && rm -rf /var/lib/apt/lists/*;

COPY ./docker/php/laravel.ini /usr/local/etc/php/conf.d/laravel.ini

WORKDIR /usr/src/app

RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

COPY ./api /usr/src/app/

RUN chown -R www:www .

RUN composer install
RUN composer dump-autoload

COPY --chown=www:www . /usr/src/app

USER www

EXPOSE 9000

CMD ["php-fpm"]