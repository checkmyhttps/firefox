FROM php:apache

RUN sed -i 's/^ServerTokens .*/ServerTokens Prod/g' /etc/apache2/conf-available/security.conf
RUN sed -i 's/^ServerSignature .*/ServerSignature Off/g' /etc/apache2/conf-available/security.conf
RUN echo 'expose_php = Off' >> /usr/local/etc/php/php.ini

RUN a2enmod ssl && service apache2 restart

COPY ./confs/apache-site.conf /etc/apache2/sites-available/cmh_api.conf
COPY ./confs/cert /usr/local/etc/cmh_cert
RUN a2dissite * && a2ensite cmh_api

RUN chmod 600 /usr/local/etc/cmh_cert/privkey.key && chown www-data:www-data /usr/local/etc/cmh_cert/privkey.key
RUN mkdir /var/tmp/cmh_cache
RUN chown www-data:www-data /var/tmp/cmh_cache/

COPY ./www /var/www/cmh_api/

RUN apt-get update && apt-get install -y \
	libicu-dev \
	&& docker-php-ext-install intl
