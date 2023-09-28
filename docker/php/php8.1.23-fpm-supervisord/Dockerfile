FROM php:8.1.23-fpm

#RUN apt-get update && apt-get install -y apt-utils wget
#
#RUN wget -O /etc/apt/sources.list https://mirrors.aliyun.com/repo/debian.list
#
## use http://mirrors.ustc.edu.cn/ sources to boost
##RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak \
##	  && cat /opt/apt-list >/etc/apt/sources.list \
##	  && apt-get update

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libzip-dev \
        libicu-dev \
        libpq-dev  \
        libevent-dev libssl-dev \
        libzip-dev zip unzip cron \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd  \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install zip intl pdo pdo_mysql bcmath opcache

# supervisord
RUN apt-cache show supervisor && apt-get update && apt-get install -y supervisor
RUN touch /var/run/supervisor.sock
RUN chmod -R 777 /var/run

RUN pecl install -o -f ev redis; \
  rm -rf /tmp/pear \
  && docker-php-ext-enable redis \
  && docker-php-ext-enable ev


# Supercronic
# Latest releases available at https://github.com/aptible/supercronic/releases
ENV SUPERCRONIC_URL=https://github.com/aptible/supercronic/releases/download/v0.2.26/supercronic-linux-amd64 \
    SUPERCRONIC=supercronic-linux-amd64 \
    SUPERCRONIC_SHA1SUM=7a79496cf8ad899b99a719355d4db27422396735

RUN curl -fsSLO "$SUPERCRONIC_URL" \
 && echo "${SUPERCRONIC_SHA1SUM}  ${SUPERCRONIC}" | sha1sum -c - \
 && chmod +x "$SUPERCRONIC" \
 && mv "$SUPERCRONIC" "/usr/local/bin/${SUPERCRONIC}" \
 && ln -s "/usr/local/bin/${SUPERCRONIC}" /usr/local/bin/supercronic

# install composer
COPY --from=composer:2.0 /usr/bin/composer /usr/local/bin/composer
RUN composer config -g repo.packagist composer https://mirrors.cloud.tencent.com/composer/
RUN composer self-update --2


# sockets
RUN CFLAGS="$CFLAGS -D_GNU_SOURCE" docker-php-ext-install sockets

## opcache
#RUN docker-php-ext-install opcache

# pcntl && event
RUN docker-php-ext-install pcntl
RUN pecl install event && \
    echo "extension=event.so" > /usr/local/etc/php/conf.d/event.ini


EXPOSE 9000


