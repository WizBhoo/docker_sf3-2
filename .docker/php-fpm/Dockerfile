FROM php:7.2-fpm-alpine

LABEL maintainer="Yann LUCAS <yann.lucas@outlook.com> for and adapted by Adrien PIERRARD <adrien.pierrard@gmail.com>"

ENV WORKDIR "/var/www"

RUN apk upgrade --update && apk --no-cache add \
    gcc g++ make git autoconf tzdata openntpd libcurl curl-dev coreutils \
    libmcrypt-dev freetype-dev libxpm-dev libjpeg-turbo-dev libvpx-dev \
    libpng-dev libxml2-dev icu-dev zsh libtool mysql-dev libzip-dev \
    nodejs nodejs-npm yarn

RUN docker-php-ext-configure intl \
    && docker-php-ext-configure opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
    --with-xpm-dir=/usr/include/

RUN docker-php-ext-install -j$(nproc) gd iconv pdo pdo_mysql curl \
    mbstring json xml xmlrpc zip bcmath intl opcache

# Install apcu
RUN pecl install apcu \
   && docker-php-ext-enable apcu

# Install imagick
RUN apk add --update imagemagick-dev \
   && pecl install imagick \
   && docker-php-ext-enable imagick

# Install xdebug (can add redis if needed)
RUN docker-php-source extract \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && docker-php-source delete

# Add timezone
RUN ln -s /usr/share/zoneinfo/UTC /etc/localtime && \
    "date"

# Install composer
RUN curl -sS https://getcomposer.org/installer | \
    php -- --install-dir=/usr/local/bin --filename=composer

# Install Symfony
RUN wget https://get.symfony.com/cli/installer -O - | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

ENV current_os=alpine
RUN version=$(php -r "echo PHP_MAJOR_VERSION.PHP_MINOR_VERSION;") \
    && curl -A "Docker" -o /tmp/blackfire-probe.tar.gz -D - -L -s https://blackfire.io/api/v1/releases/probe/php/$current_os/amd64/$version \
    && mkdir -p /tmp/blackfire \
    && tar zxpf /tmp/blackfire-probe.tar.gz -C /tmp/blackfire \
    && mv /tmp/blackfire/blackfire-*.so $(php -r "echo ini_get('extension_dir');")/blackfire.so \
    && printf "extension=blackfire.so\nblackfire.agent_socket=tcp://blackfire:8707\n" > $PHP_INI_DIR/conf.d/blackfire.ini \
    && rm -rf /tmp/blackfire /tmp/blackfire-probe.tar.gz

# Cleanup
RUN rm -rf /var/cache/apk/* \
    && find / -type f -iname \*.apk-new -delete \
    && rm -rf /var/cache/apk/*

RUN mkdir -p ${WORKDIR}

RUN set -x ; \
    delgroup www-data; \
    deluser www-data; \
    addgroup -g 1000 -S www-data ; \
    adduser -u 1000 -D -S -G www-data www-data

RUN sed -i "s|/root:/bin/ash|/root:/bin/zsh|g" /etc/passwd
RUN sed -i "s|/home/www-data:/bin/false|/home/www-data:/bin/zsh|g" /etc/passwd

# Install oh-my-zsh
RUN git clone git://github.com/robbyrussell/oh-my-zsh.git /usr/local/etc/oh-my-zsh

ADD conf/.zshrc /root/.zshrc
ADD conf/.zshrc /home/www-data/.zshrc

RUN mkdir -p /usr/local/etc/oh-my-zsh/themes \
    && wget -O /usr/local/etc/oh-my-zsh/themes/docker.zsh-theme \
    https://raw.githubusercontent.com/dpdornseifer/docker-zsh-theme/master/docker.zsh-theme

ADD conf/php.ini /usr/local/etc/php/php.ini
ADD conf/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

WORKDIR /var/www

CMD ["php-fpm"]
