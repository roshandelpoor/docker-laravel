FROM adrianharabula/php7-with-oci8

RUN printf "deb http://archive.debian.org/debian/ jessie main\ndeb-src http://archive.debian.org/debian/ jessie main\ndeb http://security.debian.org jessie/updates main\ndeb-src http://security.debian.org jessie/updates main" > /etc/apt/sources.list

RUN apt-get update -yqq && \
    apt-get install -y apt-utils zip unzip && \
    apt-get install -y nano && \
    apt-get install -y curl git nodejs npm openssh-client  && \
    apt-get install -y libzip-dev libpq-dev && \
    a2enmod rewrite && \
    docker-php-ext-install pdo_pgsql && \
    docker-php-ext-install pgsql && \
    docker-php-ext-configure zip --with-libzip && \
    docker-php-ext-install zip

RUN apt-get install -y redis-server

RUN docker-php-ext-install \
    iconv \
    mbstring \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    pcntl \
    tokenizer

RUN rm -rf /var/lib/apt/lists/*

ENV COMPOSER_HOME /composer
ENV PATH ./vendor/bin:/composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

COPY default.conf /etc/apache2/site-enabled/000-default.conf

WORKDIR /var/www/html

#CMD ["/usr/bin/redis-server","-D","FOREGROUND"]
#or
#COPY redis-start.sh /usr/bin/redis/
#ENTRYPOINT ["/usr/bin/redis/redis-start.sh"]

CMD ["/usr/sbin/apache2ctl","-D","FOREGROUND"]

EXPOSE 80


