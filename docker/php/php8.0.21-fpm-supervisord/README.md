# Dockerfile


docker build  -t php8.0.21-fpm-supervisord:1 -f ./Dockerfile  .

docker tag php8.0.21-fpm-supervisord:1  wodeaids/php:php8.0.21-fpm-supervisord

docker push wodeaids/php:php8.0.21-fpm-supervisord
