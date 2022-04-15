FROM php:7-fpm
RUN apt-get update \
 && apt-get install -y git libcurl4-gnutls-dev zlib1g-dev libicu-dev g++ libxml2-dev libpq-dev unzip vim \
 && docker-php-ext-install pdo pdo_mysql intl curl json opcache xml \
 && apt-get autoremove && apt-get autoclean \
 && rm -rf /var/lib/apt/lists/*
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_NO_INTERACTION 1
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
  && php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/local/bin/composer
RUN pecl install xdebug \
  && docker-php-ext-enable xdebug \
  && mkdir -p /var/log/xdebug
COPY php.ini /usr/local/etc/php
CMD php-fpm