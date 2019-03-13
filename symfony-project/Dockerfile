FROM php:7.2-apache
ARG TIMEZONE

RUN apt-get update && \
  apt-get install -y msmtp && \
  apt-get install -y acl && \
  apt-get install -y openssl && \
  apt-get install -y git && \
  apt-get install -y unzip && \
  apt-get clean

COPY php.ini /usr/local/etc/php/php.ini
COPY apache.conf /etc/apache2/sites-available/000-default.conf
COPY ./ /var/www/html/

RUN chown -R www-data:www-data /var/www/html/var

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql

WORKDIR /var/www/html

#ENTRYPOINT [ "./docker-entrypoint.sh" ]

