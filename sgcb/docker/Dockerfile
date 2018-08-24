FROM ubuntu:18.04

# Environment variables.
ENV LANG="en_US.UTF-8" \
    LC_ALL="en_US.UTF-8" \
    LANGUAGE="en_US.UTF-8" \
    TERM="xterm" \
    DEBIAN_FRONTEND="noninteractive"

# Expose ports and define workdir.
EXPOSE 80 443 9000
WORKDIR /app

# Install all dependencies.
RUN apt-get update -q && \
    apt-get install -qy software-properties-common language-pack-en-base && \
    export LC_ALL=en_US.UTF-8 && \
    export LANG=en_US.UTF-8 && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update -q && \
    apt-get install --no-install-recommends -qy \
        libz-dev \
        libiconv-hook1 \
        libiconv-hook-dev \
        ca-certificates \
        cron \
        curl \
        nginx \
        git \
        make \
        bzip2 \
        mysql-client \
        php5.6 \
        php5.6-bcmath \
        php5.6-common \
        php5.6-curl \
        php5.6-dom \
        php5.6-fpm \
        php5.6-gd \
        php5.6-iconv \
        php5.6-intl \
        php5.6-json \
        php5.6-mbstring \
        php5.6-mysql \
        php5.6-pdo \
        php5.6-phar \
        php5.6-sqlite \
        php5.6-xdebug \
        php5.6-xml \
        php5.6-zip \
        php5.6-memcached \
        php5.6-soap \
        php-apcu \
        php-uuid \
        supervisor \
        tzdata \
        ssmtp \
        patch \
        wget && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    cp /usr/share/zoneinfo/Pacific/Noumea /etc/localtime && echo "Pacific/Noumea" > /etc/timezone && \
    mkdir /run/php

# Add SMTP require hostnames and Drush command alias.
RUN echo "127.0.0.1 localhost localhost.localdomain $(hostname)" >> /etc/hosts \
	&& echo "alias drush='/app/vendor/drush/drush/drush'" >> $HOME/.bashrc

# Copy configurations.
COPY ssmtp.conf /etc/ssmtp/ssmtp.conf
COPY php-smtp.ini /etc/php/7.2/cli/conf.d/php-smtp.ini
COPY php.ini /etc/php/5.6/cli/conf.d/50-setting.ini
COPY php.ini /etc/php/5.6/fpm/conf.d/50-setting.ini
COPY pool.conf /etc/php/5.6/fpm/pool.d/www.conf
COPY nginx.conf /etc/nginx/nginx.conf
COPY .htpasswd /etc/nginx/.htpasswd
COPY certificates/ssl-cert-snakeoil.pem /etc/ssl/certs/ssl-cert-snakeoil.pem
COPY certificates/ssl-cert-snakeoil.key /etc/ssl/private/ssl-cert-snakeoil.key
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Launch supervisor.
CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
