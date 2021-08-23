FROM alpine:3.14.1

ARG APP_PATH=/opt/Laravel-Front
ENV APP_PATH=$APP_PATH

# Update the Base OS
RUN apk update && apk upgrade

RUN apk add nginx supervisor
RUN apk add php7 php7-fpm
RUN apk add php7-bcmath \
            php7-ctype \
            php7-curl \
            php7-dom \
            php7-exif \
            php7-fileinfo \
            php7-ftp \
            php7-gd \
            php7-gettext \
            php7-iconv \
            php7-imap \
            php7-intl \
            php7-json \
            php7-mcrypt \
            php7-mysqli \
            php7-opcache \
            php7-openssl \
            php7-pdo_mysql \
            php7-posix \
            php7-session \
            php7-simplexml \
            php7-soap \
            php7-sockets \
            php7-sodium \
            php7-tokenizer \
            php7-xml \
            php7-xmlreader \
            php7-xmlrpc \
            php7-zip \
            php7-zlib

# Uncomment to install composer
# RUN apk add composer

# Clean up after installing all required packages
RUN rm -rf /var/cache/apk/*

# Configure nginx
COPY .docker/config/nginx.conf /etc/nginx/nginx.conf

# Configure supervisord
COPY .docker/config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Configure php-fpm
COPY .docker/config/fpm-pool.conf /etc/php7/php-fpm.d/custom.conf
COPY .docker/config/php.ini /etc/php7/conf.d/custom.ini

# Copy Laravel source
COPY Laravel-Front $APP_PATH
RUN test -e $APP_PATH/storage && chown -R nobody:nobody $APP_PATH/storage

# Set environment file
COPY .env $APP_PATH/.env
RUN php $APP_PATH/artisan key:generate

# Set document root
RUN test -e /var/www/html || mkdir -p /var/www/html
RUN ln -sf $APP_PATH /var/www/html/Laravel-Front

EXPOSE 80
WORKDIR $APP_PATH
ENTRYPOINT ["/usr/bin/env"]
CMD ["sh", "-c", "export PATH=$PATH:$APP_PATH; supervisord -c /etc/supervisor/conf.d/supervisord.conf"]
