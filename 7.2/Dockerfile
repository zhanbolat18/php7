FROM php:7.2-fpm

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils \
        libbz2-dev \
        libjpeg-dev \
		libjpeg62-turbo-dev \
        libmcrypt-dev \
        libxml++2.6-dev \
        libcurl3-dev \
        libpq-dev \
        libfreetype6-dev \
        libwebp-dev \
        libpng-dev \
        libxpm-dev \
		libmcrypt-dev \
		libpng-dev \
		libmagickwand-dev \
        libmagickcore-dev \
        zlib1g-dev \
        libzip-dev \
        zip unzip python git \
	&& docker-php-ext-install curl mbstring xml zip gd \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install intl pdo mysqli pdo_mysql bcmath zip \
    && docker-php-ext-install pcntl \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && rm -rf /tmp/* /var/cache/apk/* \
    && rm -r /var/lib/apt/lists/*

#USER_ID=$(id -u ${USER})
ARG USER_ID=1000
#GROUP_ID=$(id -g ${USER})
ARG GROUP_ID=1000

RUN if [ ${USER_ID:-0} -ne 0 ] && [ ${GROUP_ID:-0} -ne 0 ]; then \
    userdel -f www-data &&\
    if getent group www-data ; then groupdel www-data; fi &&\
    groupadd -g ${GROUP_ID} www-data &&\
    useradd -l -u ${USER_ID} -g www-data www-data &&\
    install -d -m 0755 -o www-data -g www-data /home/www-data &&\
    chown --changes --silent --no-dereference --recursive \
          --from=33:33 ${USER_ID}:${GROUP_ID} \
        /home/www-data \
;fi

RUN  curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

USER www-data

RUN composer global require hirak/prestissimo

ENV PATH=${PATH}:/home/www-data/.composer/vendor/bin

WORKDIR /var/www