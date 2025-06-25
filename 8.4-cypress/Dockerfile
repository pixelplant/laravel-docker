FROM ubuntu:22.04
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y software-properties-common && add-apt-repository ppa:ondrej/php

RUN apt update && \
    apt upgrade -y && \
    apt-get install -y openssl php8.4

RUN apt-get install -y -f apt-transport-https \
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
        libcurl4-openssl-dev \
        libgtk2.0-0 \
        libgtk-3-0 \
        libgbm-dev \
        libnotify-dev \
        libnss3 \
        libxss1 \
        libasound2 \
        libxtst6 \
        xauth \
        xvfb \
        python3 \
        python3-pip \
        ipython3 \
        php8.4-imagick

RUN pip install numpy
RUN pip install scipy
RUN pip install pandas

# Install node 16 and npm 8
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
        php8.4-zip

RUN curl -sS https://getcomposer.org/installer | php \
	&& mv composer.phar /usr/local/bin/composer \
	&& composer self-update

# Install Chrome
RUN apt-get install -y wget
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt install -y ./google-chrome-stable_current_amd64.deb


# wkhtmltopdf for ubuntu
RUN apt-get update && apt-get -y install wkhtmltopdf && ln -s /usr/bin/wkhtmltopdf /usr/local/bin/wkhtmltopdf

# Install PHP_CodeSniffer
RUN apt-get install -y php-codesniffer

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
