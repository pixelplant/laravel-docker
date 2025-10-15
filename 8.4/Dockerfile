FROM ubuntu:22.04
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y software-properties-common && add-apt-repository ppa:ondrej/php

RUN apt-get update && apt-get install -y \
  openssl \
  php8.4

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

# Install node 20
RUN curl -s https://deb.nodesource.com/setup_20.x | bash && apt-get install nodejs -y

RUN apt-get update && apt-get install -y \
        php8.4-bcmath \
        php8.4-calendar \
        php8.4-curl \
        php8.4-exif \
        php8.4-gd \
        php8.4-iconv \
        php8.4-intl \
        php8.4-mbstring \
        php8.4-pdo \
        php8.4-pdo-pgsql \
        php8.4-pgsql \
        php8.4-tokenizer \
        php8.4-xml \
        php8.4-sockets \
        php8.4-zip \
        php8.4-imagick

RUN curl -sS https://getcomposer.org/installer | php \
	&& mv composer.phar /usr/local/bin/composer \
	&& composer self-update

RUN apt-get install -y openvpn

# Enable PECL and PEAR extensions
#RUN docker-php-ext-enable \
#    imagick

# Install Chrome
RUN apt-get install -y wget
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt install -y ./google-chrome-stable_current_amd64.deb

# Lighthouse
#RUN apt-get update && apt-get install -y \
#    xvfb \
#    && npm i -g lighthouse

# Install Chrome headless shell
RUN wget https://storage.googleapis.com/chrome-for-testing-public/142.0.7444.6/linux64/chrome-headless-shell-linux64.zip
RUN unzip chrome-headless-shell-linux64.zip

# wkhtmltopdf for ubuntu
RUN apt-get update && apt-get -y install wkhtmltopdf && ln -s /usr/bin/wkhtmltopdf /usr/local/bin/wkhtmltopdf

# Install PHP_CodeSniffer
RUN apt-get install -y php-codesniffer

# Install postgresql 16
RUN sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
RUN curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
RUN apt-get update && apt-get -y install postgresql-16 postgresql-client-16

# Setup working directory
WORKDIR /var/www

USER postgres

RUN pg_ctlcluster 16 main start && pg_ctlcluster 16 main status && \
    psql --command "CREATE USER homestead WITH SUPERUSER PASSWORD 'secret';" && \
    createdb -O homestead neho_test

RUN echo "host all  all    127.0.0.1/32  md5" >> /etc/postgresql/16/main/pg_hba.conf && \
    echo "listen_addresses='*'" >> /etc/postgresql/16/main/postgresql.conf

USER root

#ENTRYPOINT ["/bin/bash", "-c", "service postgresql start && service postgresql status && sleep infinity"]
