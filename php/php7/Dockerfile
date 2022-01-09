FROM alpine:3.8

LABEL maintainer="Juliano Petronetto <juliano@petronetto.com.br>"


RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories

ENV BUILD_DEPS="autoconf g++ make php7-dev php7-pear tzdata  zlib-dev" \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    PHP_IGBINARY_COMPACT_STRINGS=On \
    PHP_IGBINARY_VERSION=2.0.8 \
    PHP_OAUTH_VERSION=2.0.3 \
    PHP_SYSCONF_PATH=/etc/php7 \
# PHP Configs
    PHP_MEMORY_LIMIT=256M \
    PHP_PRECISION=-1 \
    PHP_OUTPUT_BUFFERING=4096 \
    PHP_SERIALIZE_PRECISION=-1 \
    PHP_MAX_EXECUTION_TIME=60 \
    PHP_ERROR_REPORTING=E_ALL \
    PHP_DISPLAY_ERRORS=0 \
    PHP_DISPLAY_STARTUP_ERRORS=0 \
    PHP_TRACK_ERRORS=0 \
    PHP_LOG_ERRORS=1 \
    PHP_LOG_ERRORS_MAX_LEN=10240 \
    PHP_ACCESS_LOG=/var/log/php/access.log \
    PHP_POST_MAX_SIZE=20M \
    PHP_MAX_UPLOAD_FILESIZE=10M \
    PHP_MAX_FILE_UPLOADS=20 \
    PHP_MAX_INPUT_TIME=60 \
    PHP_VARIABLES_ORDER=EGPCS \
    PHP_REQUEST_ORDER=GP \
    PHP_OPCACHE_ENABLE=0 \
    PHP_OPCACHE_ENABLE_CLI=0 \
    PHP_OPCACHE_MEMORY_CONSUMPTION=128 \
    PHP_OPCACHE_INTERNED_STRINGS_BUFFER=32 \
    PHP_OPCACHE_MAX_ACCELERATED_FILES=10000 \
    PHP_OPCACHE_USE_CWD=1 \
    PHP_OPCACHE_VALIDATE_TIMESTAMPS=1 \
    PHP_OPCACHE_REVALIDATE_FREQ=2 \
    PHP_OPCACHE_ENABLE_FILE_OVERRIDE=0 \
    PHP_ZEND_ASSERTIONS=-1 \
    PHP_PM_ERROR_LOG=/var/log/php/fpm-error.log \
    PHP_PM=ondemand \
    PHP_PM_MAX_CHILDREN=100 \
    PHP_PM_START_SERVERS=20 \
    PHP_PM_MIN_SPARE_SERVERS=20 \
    PHP_PM_MAX_SPARE_SERVERS=20 \
    PHP_PM_PROCESS_IDLE_TIMEOUT=60s \
    PHP_PM_MAX_REQUESTS=500

# Copying the config files
COPY ./ /app

# Installing necessary packages
RUN set -ex; \
    apk --update upgrade --no-cache; \
    apk add --no-cache --virtual .build-deps ${BUILD_DEPS}; \
    apk add --no-cache \
        git \
        curl \
        zlib \
        ca-certificates \
        openssl \
        php7 \
        php7-dom \
        php7-fpm \
        php7-cgi \
        php7-gd \
        php7-intl \
        php7-iconv \
        php7-mbstring \
        php7-mcrypt \
        php7-pdo \
        php7-pdo_mysql \
        php7-pdo_pgsql \
        php7-pdo_sqlite \
        php7-fileinfo \
        php7-xml \
        php7-simplexml \
        php7-xmlreader \
        php7-xmlwriter \
        php7-phar \
        php7-openssl \
        php7-json \
        php7-curl \
        php7-ctype \
        php7-session \
        php7-zlib \
        php7-tokenizer; \
# Configuring timezones
    cp /usr/share/zoneinfo/Etc/UTC /etc/localtime; \
    echo "UTC" | tee /etc/timezone; \
# install PHP driver
    git clone --depth=1 -b ${PHP_IGBINARY_VERSION} https://github.com/igbinary/igbinary.git /tmp/php-igbinary && \
    cd /tmp/php-igbinary && \
    phpize && ./configure CFLAGS="-O2 -g" --enable-igbinary --with-php-config=/usr/bin/php-config7 && make && make install && \
    cd .. && rm -rf /tmp/php-igbinary/ && \
    echo 'extension=igbinary.so' >> ${PHP_SYSCONF_PATH}/conf.d/igbinary.ini \
    ; \
    cd /tmp && wget https://pecl.php.net/get/oauth-${PHP_OAUTH_VERSION}.tgz && tar xvzf oauth-${PHP_OAUTH_VERSION}.tgz && \
    cd oauth-${PHP_OAUTH_VERSION} && \
    phpize && ./configure --with-php-config=/usr/bin/php-config7 && make && make install && \
    cd .. && rm -rf /tmp/${PHP_OAUTH_VERSION}/ && \
    echo 'extension=oauth.so' >> ${PHP_SYSCONF_PATH}/conf.d/oauth.ini \
    ; \
    git clone --depth=1 -b ${PHP_REDIS_VERSION} https://github.com/phpredis/phpredis.git /tmp/phpredis && \
    cd /tmp/phpredis && phpize &&  ./configure &&  make && make install && \
    cd .. && rm -rf /tmp/phpredis && \
    echo 'extension=redis.so' >> ${PHP_SYSCONF_PATH}/conf.d/redis.ini \
    ; \
    rm -rf /tmp/pear; \
# Create application folder
    mkdir -p /app; \
# Creating www-data user and group
    addgroup -g 1000 -S www-data; \
	adduser -u 1000 -D -S -h /app -s /sbin/nologin -G www-data www-data; \
# Installing Composer
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer; \
# Compying PHP config files
    cp -a /app/config/php.ini /etc/php7/php.ini; \
    cp -a /app/config/www.conf /etc/php7/php-fpm.d/www.conf; \
    cp -a /app/config/php-fpm.conf /etc/php7/php-fpm.conf; \
# Cleaning
    apk del .build-deps; \
    rm -rf /var/cache/apk/*; \
    rm -rf /app/config;

WORKDIR /app

# The container shouldn't run as root, it can be a potential
# security issue, obviously, depending the enviroment
# but to ensure the security, we aren't going to run as root.
USER www-data

CMD ["/usr/sbin/php-fpm7"]