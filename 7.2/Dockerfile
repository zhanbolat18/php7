FROM php:7.2-fpm

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils \
        libmcrypt-dev \
        libxml++2.6-dev \
        libcurl3-dev \
        libpq-dev \
        libfreetype6-dev \
        libwebp-dev \
		libjpeg62-turbo-dev \
        libpng-dev \
        libxpm-dev \
		libmcrypt-dev \
		libpng-dev \
		libmagickwand-dev \
        libmagickcore-dev \
        zlib1g-dev \
        libzip-dev \
        zip unzip python git \
	&& docker-php-ext-install curl mbstring xml zip \
    && docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install intl pdo pgsql pdo_pgsql bcmath zip \
    && rm -rf /tmp/* /var/cache/apk/* \
    && rm -r /var/lib/apt/lists/* \
    && pecl install

#USER_ID=$(id -u ${USER})
ARG USER_ID
#GROUP_ID=$(id -g ${USER})
ARG GROUP_ID

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

RUN  curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
        && composer global require hirak/prestissimo

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        libbz2-dev \
        libfreetype6-dev \
        libjpeg-dev \
        libpng-dev \
        libwebp-dev \
        libxpm-dev \
    && docker-php-ext-install gd

RUN apt-get update && docker-php-ext-install pcntl

USER www-data

WORKDIR /var/www

