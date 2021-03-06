FROM php:7.1-apache

RUN a2enmod rewrite

RUN set -xe \
&& apt-get update \
&& apt-get install -y libpng-dev libjpeg-dev libpq-dev libxml2-dev \
&& docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
&& docker-php-ext-install gd mbstring mysqli pgsql soap \
&& rm -rf /var/lib/apt/lists/*

ENV MANTIS_VER 2.19.0
ENV MANTIS_MD5 2e9ccefa8be801db09ddd2461b4521c4
ENV MANTIS_URL http://jaist.dl.sourceforge.net/project/mantisbt/mantis-stable/${MANTIS_VER}/mantisbt-${MANTIS_VER}.tar.gz
ENV MANTIS_FILE mantisbt.tar.gz
ENV MANTIS_TIMEZONE America/Sao_Paulo

RUN curl -fSL ${MANTIS_URL} -o ${MANTIS_FILE} \
&& echo "${MANTIS_MD5} ${MANTIS_FILE}" | md5sum -c \
&& mkdir -p /var/www/html \
&& tar -xz --strip-components=1 -f ${MANTIS_FILE} -C /var/www/html \
&& rm ${MANTIS_FILE} \
&& chown -R www-data:www-data /var/www/html \
&& mkdir /opt/files \
&& chown -R www-data:www-data /opt/files \
&& chmod 700 /opt/files

RUN set -xe \
&& ln -sf /usr/share/zoneinfo/${MANTIS_TIMEZONE} /etc/localtime \
&& echo 'date.timezone = "${MANTIS_TIMEZONE}"' >> /usr/local/etc/php/php.ini \
&& echo 'upload_max_filesize=30M' >> /usr/local/etc/php/php.ini \
&& echo 'post_max_size=32M' >> /usr/local/etc/php/php.ini

CMD ["apache2-foreground"]
