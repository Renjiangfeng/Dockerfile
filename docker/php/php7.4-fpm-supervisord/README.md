# Dockerfile


docker build  -t php7.4.28-fpm-supervisord:1 -f ./Dockerfile  .

docker tag php7.4.28-fpm-supervisord:1  wodeaids/php:php7.4.28-fpm-supervisord

docker push wodeaids/php:php7.4.28-fpm-supervisord
