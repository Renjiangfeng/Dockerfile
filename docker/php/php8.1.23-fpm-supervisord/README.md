# Dockerfile


docker build  -t php8.1.23-fpm-supervisord:1 -f ./Dockerfile  .

docker tag php8.1.23-fpm-supervisord:1  wodeaids/php:php8.1.23-fpm-supervisord

docker push wodeaids/php:php8.1.23-fpm-supervisord
