FROM php:7-fpm-alpine

# Install all dependencies.
RUN apk --no-cache update \
    && apk --no-cache upgrade \
    && apk add --no-cache $PHPIZE_DEPS \
        ca-certificates \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        postgresql-dev \
        curl \
        git \
        tzdata \
        ssmtp \
        patch && \
    pecl install xdebug-2.6.0 && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure opcache --enable-opcache && \
    docker-php-ext-install -j$(getconf _NPROCESSORS_ONLN) gd pdo_pgsql opcache && \
    docker-php-ext-enable xdebug && \
    cp /usr/share/zoneinfo/Pacific/Noumea /etc/localtime && echo "Pacific/Noumea" > /etc/timezone && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    curl -L -o /tmp/nodejs.tar.gz https://nodejs.org/dist/v10.3.0/node-v10.3.0-linux-x64.tar.gz && \
    tar xfvz /tmp/nodejs.tar.gz -C /usr/local --strip-components=1 && \
    rm -f /tmp/nodejs.tar.gz && \
    curl -sS http://get.sensiolabs.org/security-checker.phar > /usr/local/bin/security-checker && \
    chmod +x /usr/local/bin/security-checker && \
    mkdir /run/php && \
    echo "127.0.0.1 localhost localhost.localdomain $(hostname)" >> /etc/hosts && \
    echo "alias drush='/app/vendor/drush/drush/drush'" >> $HOME/.bashrc

# Copy configurations.
COPY ssmtp.conf /etc/ssmtp/ssmtp.conf
COPY php-smtp.ini /etc/php/7.2/cli/conf.d/php-smtp.ini
COPY php.ini /etc/php/7.2/cli/conf.d/50-setting.ini
COPY php.ini /etc/php/7.2/fpm/conf.d/50-setting.ini
COPY pool.conf /etc/php/7.2/fpm/pool.d/www.conf

# Define workdir.
WORKDIR /app

# Run php-fpm.
EXPOSE 9000
CMD ["php-fpm"]
