FROM php:7-fpm-alpine

# Install all dependencies.
RUN apk --no-cache update \
    && apk --no-cache upgrade \
    && apk add --no-cache $PHPIZE_DEPS \
        ca-certificates \
        postgresql-dev \
        postgresql-client \
        libmemcached-dev \
        zlib-dev \
        libxml2-dev \
        curl \
        git \
        tzdata \
        ssmtp \
        patch \
        nodejs \
        nodejs-npm \
    && pecl install xdebug-2.6.0 \
    && pecl install memcached-3.0.4 \
    && docker-php-ext-enable xdebug memcached \
    && docker-php-ext-install xml \
    && docker-php-ext-install soap \
    && docker-php-ext-install pdo_pgsql \
    && apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-configure gd \
            --with-freetype-dir=/usr/include/ \
            --with-png-dir=/usr/include/ \
            --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install -j$(nproc) opcache \
    && cp /usr/share/zoneinfo/Pacific/Noumea /etc/localtime && echo "Pacific/Noumea" > /etc/timezone \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && curl -sS http://get.sensiolabs.org/security-checker.phar > /usr/local/bin/security-checker \
    && chmod +x /usr/local/bin/security-checker \
    && mkdir /run/php \
    && npm config set unsafe-perm true \
    && npm install --global gulp-cli \
    && echo "127.0.0.1 localhost localhost.localdomain $(hostname)" >> /etc/hosts \
    && echo "alias ll='ls -l'" > /etc/profile.d/ll.sh \
    && echo "alias drush='/app/vendor/drush/drush/drush'" > /etc/profile.d/drush.sh \
    && apk del --no-cache freetype-dev libpng-dev libjpeg-turbo-dev

# Copy configurations.
COPY ssmtp.conf /etc/ssmtp/ssmtp.conf
COPY php.ini /usr/local/etc/php/conf.d/php-smtp.ini
COPY php.ini /usr/local/etc/php/conf.d/50-setting.ini
COPY pool.conf /usr/local/etc/php-fpm.d/www.conf

# Define workdir.
WORKDIR /app

# Run php-fpm.
EXPOSE 9000
CMD ["php-fpm"]
