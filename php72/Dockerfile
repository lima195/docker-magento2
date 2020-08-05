FROM php:7.2-fpm

RUN useradd -m john && echo "john:john" | chpasswd && adduser john sudo && adduser john www-data

ARG DEBIAN_FRONTEND=noninteractive

ENV HOME /home/john

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

# Get repository and install wget and vim
RUN apt-get update && apt-get install --no-install-recommends -y \
        wget \
        vim \
        git \
        apt-utils \
        unzip

# Install PHP extensions deps
RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        zlib1g-dev \
        libicu-dev \
        g++ \
        unixodbc-dev \
        libxml2-dev \
        libaio-dev \
        freetds-dev \
		libssl-dev \
        libxslt-dev \
		openssl

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php \
	&& mv composer.phar /usr/local/bin/composer \
	&& composer global require hirak/prestissimo --no-plugins --no-scripts

# Install PHP extensions
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install \
            iconv \
            mbstring \
            bcmath \
            xsl \
            intl \
            gd \
            mysqli \
            pdo_mysql \
            soap \
            zip \
            xmlrpc \
            opcache 

RUN apt-get update && apt-get install -y libmcrypt-dev \
        && pecl install mcrypt-1.0.2 \
        && docker-php-ext-enable mcrypt \
        && docker-php-ext-enable bcmath

# Install Xdebug
RUN apt-get update \
    && pecl install -f xdebug \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_handler=dbgp" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_port=9001" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_connect_back=0" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.idekey=docker" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_host=172.22.0.1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Clean repository
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/*

COPY ./etc/php_ini/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY ./etc/php_ini/php.ini /usr/local/etc/php/php.ini

## Fixing owner permissions
RUN chown -R john:john $HOME

USER john