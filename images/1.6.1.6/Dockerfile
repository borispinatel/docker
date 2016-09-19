FROM php:5.6-apache

MAINTAINER Boris Pinatel <boris.pinatel@orange.com>

ENV PS_LANGUAGE en
ENV PS_COUNTRY gb
ENV PS_INSTALL_AUTO 0
ENV PS_DEV_MODE 0
ENV PS_HOST_MODE 0
ENV PS_HANDLE_DYNAMIC_DOMAIN 0

ENV PS_FOLDER_ADMIN admin
ENV PS_FOLDER_INSTALL install

# needed for Apache2 under Ubuntu
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

ENV PS_VERSION 1.6.1.6
ENV PS_URL https://www.prestashop.com/ajax/controller.php?method=download&type=releases&file=prestashop_1.6.1.6.zip&language=fr

ENV PS_DOMAIN prestashop.local

# Get PrestaShop
ADD $PS_URL /tmp/prestashop.zip
RUN unzip -q /tmp/prestashop.zip -d /tmp/ && mv /tmp/prestashop/* /var/www/html && rm /tmp/prestashop.zip
COPY config_files/docker_updt_ps_domains.php /var/www/html/

# Change folder location and rights for logs
RUN a2enmod rewrite
RUN chmod -R a+rwx /var/www/html/
RUN sed -e 's/Listen 80/Listen 8080/' -i /etc/apache2/apache2.conf /etc/apache2/ports.conf \
 && sed -i 's/ErrorLog .*/ErrorLog \/var\/log\/apache2\/error.log/' /etc/apache2/apache2.conf \
 && sed -i 's/CustomLog .*/CustomLog \/var\/log\/apache2\/custom.log combined/' /etc/apache2/apache2.conf \
 && sed -i 's/LogLevel .*/LogLevel info/' /etc/apache2/apache2.conf \
 && touch /var\/log\/apache2\/error.log \
 && touch \/var\/log\/apache2\/custom.log \
 && chmod -R a+rwx /var/log/apache2 \
 && chmod -R a+rwx /var/lock/apache2 \
 && chmod -R a+rwx /var/run/apache2

# needed for Apache2 under Ubuntu
RUN mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR

# Expose port 8080 because 80 is allowed only for root 
EXPOSE 8080

# PHP configuration
COPY config_files/php.ini /usr/local/etc/php/

VOLUME /var/www/html/modules
VOLUME /var/www/html/themes
VOLUME /var/www/html/override

RUN chmod +x /tmp/docker_run.sh

#User
USER 1001
ENTRYPOINT ["/tmp/docker_run.sh"]
