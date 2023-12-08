FROM php:8.1-fpm

# Install dockerize so we can wait for containers to be ready
ENV DOCKERIZE_VERSION 0.6.1

RUN curl -s -f -L -o /tmp/dockerize.tar.gz https://github.com/jwilder/dockerize/releases/download/v$DOCKERIZE_VERSION/dockerize-linux-amd64-v$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf /tmp/dockerize.tar.gz \
    && rm /tmp/dockerize.tar.gz


# Install Composer
ENV COMPOSER_VERSION 2.1.5

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=$COMPOSER_VERSION

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libz-dev \
        libpq-dev \
        libssl-dev \
        libzip-dev \
        libxml2-dev \
        libxml2 \
        libfreetype6-dev libjpeg62-turbo-dev libpng-dev libjpeg-dev libwebp-dev \
        jpegoptim optipng pngquant gifsicle \
#        build-essential \
        libmcrypt-dev \
        libgd-dev \
        libcurl4-openssl-dev \
        libonig-dev \
        curl \
        unzip \
        zip \
        ca-certificates curl gnupg \
    && apt-get clean \
    && docker-php-ext-configure gd \
          --with-freetype=/usr/include/ \
          --with-jpeg=/usr/lib/x86_64-linux-gnu \
          --with-webp=/usr/lib/x86_64-linux-gnu \
    && docker-php-ext-install \
        gd \
        exif \
        xml \
        curl \
        opcache \
        intl \
        pdo_mysql \
        pdo \
        pcntl \
        zip \
        intl \
        mysqli \
        mbstring \
    && docker-php-ext-enable gd \
    && rm -rf /var/lib/apt/lists/*;

# Install nodejs, npm
RUN mkdir -p /etc/apt/keyrings
RUN curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg

ENV NODE_MAJOR=20
RUN echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
RUN apt-get update && apt-get install nodejs -y -f

# Setup php ini file
#COPY ./docker/php/my.ini /usr/local/etc/php/conf.d/my.ini
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"


WORKDIR /usr/src/app

RUN chown -R www-data:www-data .
