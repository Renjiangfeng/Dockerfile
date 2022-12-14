FROM php:8.0.21-fpm

ADD apt-list /opt/

# use http://mirrors.ustc.edu.cn/ sources to boost
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak \
	  && cat /opt/apt-list >/etc/apt/sources.list \
	  && apt-get update

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
    && docker-php-ext-install zip intl pdo pdo_mysql

# supervisord
RUN apt-cache show supervisor && apt-get update && apt-get install -y supervisor
RUN touch /var/run/supervisor.sock
RUN chmod -R 777 /var/run

RUN pecl install -o -f ev redis; \
  rm -rf /tmp/pear \
  && docker-php-ext-enable redis \
  && docker-php-ext-enable ev


# Supercronic
ENV SUPERCRONIC_URL=https://github.com/aptible/supercronic/releases/download/v0.1.12/supercronic-linux-amd64 \
  SUPERCRONIC=supercronic-linux-amd64 \
  SUPERCRONIC_SHA1SUM=048b95b48b708983effb2e5c935a1ef8483d9e3e
RUN curl -fsSLO "$SUPERCRONIC_URL" \
 && echo "${SUPERCRONIC_SHA1SUM}  ${SUPERCRONIC}" | sha1sum -c - \
 && chmod +x "$SUPERCRONIC" \
 && mv "$SUPERCRONIC" "/usr/local/bin/${SUPERCRONIC}" \
 && ln -s "/usr/local/bin/${SUPERCRONIC}" /usr/local/bin/supercronic

# sockets
RUN CFLAGS="$CFLAGS -D_GNU_SOURCE" docker-php-ext-install sockets

# opcache
RUN docker-php-ext-install opcache

# pcntl && event
RUN docker-php-ext-install pcntl
RUN pecl install event && \
    echo "extension=event.so" > /usr/local/etc/php/conf.d/event.ini


EXPOSE 9000


