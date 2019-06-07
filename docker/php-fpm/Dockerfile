FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
    libgmp-dev  \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    git \
    libzip-dev \
    zip \
    unzip \
    libxml2-dev \
    libgmp-dev re2c libmhash-dev libmcrypt-dev file

RUN ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include/

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-configure zip --with-libzip \
  && docker-php-ext-configure gmp \
  && docker-php-ext-install -j$(nproc) gd \
  && docker-php-ext-enable gd \
  && docker-php-ext-install pdo_mysql zip \ 
  && docker-php-ext-install gmp

#install composer
RUN curl --silent --show-error https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

#install nvm
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 10.15.0
ENV NVM_INSTALL_PATH $NVM_DIR/versions/node/v$NODE_VERSION
RUN mkdir /usr/local/nvm \
    && rm /bin/sh && ln -s /bin/bash /bin/sh \
    && curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

ENV NODE_PATH $NVM_INSTALL_PATH/lib/node_modules
ENV PATH $NVM_INSTALL_PATH/bin:$PATH

RUN usermod -u 1000 www-data
COPY --chown=www-data:www-data . /var/www/
USER www-data

EXPOSE 9000