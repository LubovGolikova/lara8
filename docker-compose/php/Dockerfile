FROM php:8.0-apache

RUN groupadd --gid 1000 developer && useradd -m -G www-data -u 1000 --gid=1000 developer
ENV APACHE_RUN_USER=developer

RUN apt-get update && apt-get install -y \
        libpng-dev \
        zlib1g-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        zip \
        curl \
        unzip \
    && docker-php-ext-configure gd \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-source delete
#    # Install Composer
#    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
#    && composer self-update --1 \
#    && composer global require hirak/prestissimo --no-plugins --no-scripts \
    # Install NodeJS
#    && curl --silent --location https://rpm.nodesource.com/setup_12.x | bash - \
#    && yum -y --setopt=tsflags=nodocs install nodejs

## Get latest Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"	&& \
php composer-setup.php && \
php -r "unlink('composer-setup.php');"


RUN chown -R 777 /var/www \
    && chmod 777 /var/www \
    && a2enmod rewrite
COPY httpd.conf  /etc/apache2/sites-enabled/000-default.conf
