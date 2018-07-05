FROM php:5.6-fpm



#Instalación Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version
RUN curl https://phar.phpunit.de/phpunit-7.phar > phpunit.phar && \
chmod +x phpunit.phar &&  cp phpunit.phar /usr/local/lib/php



#Setear TimeZone.
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone ="%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"
#Configuración XDEBug

RUN pecl install xdebug-2.5.5 && docker-php-ext-enable xdebug
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/php.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/php.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/php.ini
RUN echo 'zend_extension="/usr/local/lib/php/extensions/no-debug-non-zts-20151012/xdebug.so"' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_port=9000' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_enable=1' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_connect_back=1' >> /usr/local/etc/php/php.ini

#RUN apt-get update
RUN apt-get update
RUN apt-get install -y zlib1g-dev
RUN apt-get install -y libpq-dev
RUN apt-get install -y libc-client-dev libkrb5-dev
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl && docker-php-ext-install imap

# Install Postgre PDO
RUN apt-get install -y libxml2-dev
RUN apt-get install -y libmcrypt-dev
RUN apt-get install -y libcurl4-openssl-dev

RUN apt-get install -y libpq-dev \
   && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
   && docker-php-ext-install pdo pdo_pgsql pgsql \
   && docker-php-ext-install pdo pdo_mysql mysql \
   && docker-php-ext-install mbstring \
   && docker-php-ext-install zip \
   && docker-php-ext-install xmlrpc \
   && docker-php-ext-install curl \
   && docker-php-ext-install soap \
   && docker-php-ext-install bcmath \
   && docker-php-ext-install dba \
   && docker-php-ext-install mcrypt


# Default PHP config
#RUN if [ -f /usr/local/etc/php/php.ini ]; then rm /usr/local/etc/php/php.ini; fi
#COPY ["conf/php.ini", "/usr/local/etc/php/"]

