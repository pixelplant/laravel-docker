FROM ubuntu:20.04

RUN apt-get update && apt-get install -y software-properties-common && add-apt-repository ppa:ondrej/php

RUN apt-get update && apt-get install -y \
  openssl \
  php8.0

RUN apt-get update \
  && apt-get install -y -f apt-transport-https \
		libicu-dev \
		libjpeg-dev \
		libfreetype6-dev \
		libonig-dev \
		libpng-dev \
		libpq-dev \
		libwebp-dev \
		libxml2-dev \
		libzip-dev \
        rsync \
		acl \
		cron \
		git \
		zip \
        curl \
        libcurl4-openssl-dev

# Install node 16 and npm 8
RUN curl -s https://deb.nodesource.com/setup_16.x | bash && apt-get install nodejs -y

RUN apt-get update && apt-get install -y \
        php8.0-bcmath \
        php8.0-calendar \
        php8.0-curl \
        php8.0-exif \
        php8.0-gd \
        php8.0-iconv \
        php8.0-intl \
        php8.0-mbstring \
        php8.0-pdo \
        php8.0-pdo-pgsql \
        php8.0-pgsql \
        php8.0-tokenizer \
        php8.0-xml \
        php8.0-sockets \
        php8.0-zip

RUN curl -sS https://getcomposer.org/installer | php \
	&& mv composer.phar /usr/local/bin/composer \
	&& composer self-update        

# Enable PECL and PEAR extensions
#RUN docker-php-ext-enable \
#    imagick
	
# Install Selenium
# RUN apt-get update && apt-get install -y default-jre
# RUN apt-get update && apt-get install -y chromium-driver wget 

# #RUN npm install chromedriver
# RUN wget https://selenium-release.storage.googleapis.com/3.141/selenium-server-standalone-3.141.59.jar \
# 	&& mv -f selenium-server-standalone-3.141.59.jar /usr/local/bin/selenium-server-standalone.jar \
# 	&& chown root:root /usr/local/bin/selenium-server-standalone.jar \
# 	&& chmod 0755 /usr/local/bin/selenium-server-standalone.jar

# Install Chrome
RUN apt-get install -y wget
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt install -y ./google-chrome-stable_current_amd64.deb    

#java -Dwebdriver.chrome.driver="/usr/bin/chromedriver" -jar /usr/local/bin/selenium-server-standalone.jar
	
# Lighthouse
#RUN apt-get update && apt-get install -y \
#    xvfb \
#    && npm i -g lighthouse

# wkhtmltopdf for ubuntu
RUN curl "https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.focal_amd64.deb" -L -o "wkhtmltopdf.deb"
RUN apt-get update && apt-get install -y fontconfig xfonts-75dpi xfonts-base
RUN dpkg -i wkhtmltopdf.deb

# Install PHP_CodeSniffer
RUN apt-get install -y php-codesniffer

# Install postgresql 12
RUN apt-get update && apt-get -y install postgresql-12 postgresql-client-12

# Setup working directory
WORKDIR /var/www

USER postgres

RUN pg_ctlcluster 12 main start && pg_ctlcluster 12 main status && \
    psql --command "CREATE USER homestead WITH SUPERUSER PASSWORD 'secret';" && \
    createdb -O homestead neho_test

RUN echo "host all  all    127.0.0.1/32  md5" >> /etc/postgresql/12/main/pg_hba.conf && \
    echo "listen_addresses='*'" >> /etc/postgresql/12/main/postgresql.conf
	
USER root

#ENTRYPOINT ["/bin/bash", "-c", "service postgresql start && service postgresql status && java -Dwebdriver.chrome.driver=/usr/bin/chromedriver -jar /usr/local/bin/selenium-sver-standalone.jar && sleep infinity"]
#ENTRYPOINT ["/bin/bash", "-c", "service postgresql start && service postgresql status && sleep infinity"]
