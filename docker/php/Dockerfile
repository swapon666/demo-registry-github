FROM php:8.1-fpm-bullseye

RUN apt-get update && apt-get install -y curl ca-certificates gnupg \
    && curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor | tee /etc/apt/trusted.gpg.d/apt.postgresql.org.gpg >/dev/null \
    && echo "deb http://apt.postgresql.org/pub/repos/apt bullseye-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
# Install packages
    && apt-get update && apt-get install -y --no-install-recommends \
    libpq5 \
    postgresql-client-14 \
    && rm -rf /var/lib/apt/lists/* && apt-get purge -y --auto-remove \
    && mkdir -p /var/log/supervisor \
    ;

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    libpq-dev \
    librabbitmq-dev \
    libssh-dev \
    unzip \
    wget \
    zip \
    libicu-dev \
    && rm -rf /var/lib/apt/lists/* && apt-get purge -y --auto-remove \
    ;

RUN docker-php-ext-install \
    pdo_pgsql \
    sockets \
    bcmath \
    intl \
    ;

RUN pecl install \
    amqp \
    && docker-php-ext-enable \
    amqp \
    sockets \
    ;

RUN echo "$(curl -sS https://composer.github.io/installer.sig) -" > composer-setup.php.sig \
        && curl -sS https://getcomposer.org/installer | tee composer-setup.php | sha384sum -c composer-setup.php.sig \
        && php composer-setup.php && rm composer-setup.php* \
        && chmod +x composer.phar && mv composer.phar /usr/bin/composer

COPY docker/php/symfony.ini /usr/local/etc/php-fpm.d/
COPY docker/php/others.ini /usr/local/etc/php-fpm.d/

#COPY docker/php/symfony.pool.conf /etc/php8/php-fpm.d/

env UID=1000
env GID=1000

#RUN groupadd -g $GID local \
#    && useradd -rm -d /home/local -s /bin/bash -g $GID -G sudo -u $UID local \
#    && chown -R $UID:$GID /var/www

CMD ["php-fpm", "-F"]

WORKDIR /var/www/html
EXPOSE 9001
