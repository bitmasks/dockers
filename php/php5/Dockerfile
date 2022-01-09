FROM php:5.6-fpm
LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"


# Installing tools and PHP extentions using "apt", "docker-php", "pecl"

# Install  software
RUN set -eux; \
    apt-get update; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends \
            curl \
            libmemcached-dev \
            libz-dev \
            libpq-dev \
            libjpeg-dev \
            libpng-dev \
            libfreetype6-dev \
            libssl-dev \
            libmcrypt-dev; \
    # cleanup
    rm -rf /var/lib/apt/lists/*


# Install the PHP  extention
RUN set -eux; \
    # Install the PHP mcrypt extention
    docker-php-ext-install mcrypt; \
    # Install the PHP pdo_mysql extention
    docker-php-ext-install pdo_mysql mysqli; \
    # Install the PHP pdo_pgsql extention
    docker-php-ext-install pdo_pgsql; \
    # Install the PHP gd library
    docker-php-ext-configure gd \
            --enable-gd-native-ttf \
            --with-jpeg-dir=/usr/lib \
            --with-freetype-dir=/usr/include/freetype2; \
    docker-php-ext-install gd; \
    php -r 'var_dump(gd_info());' \


RUN   cd /var/tmp  ; \
      php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"  ; \
      php composer-setup.php  ;\
      php -r "unlink('composer-setup.php');"  ;\
      mv composer.phar /usr/local/bin/composer

