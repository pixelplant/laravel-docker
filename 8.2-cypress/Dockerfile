FROM ubuntu:20.04

RUN apt-get update && apt-get install -y software-properties-common && add-apt-repository ppa:ondrej/php

RUN apt-get update && apt-get install -y \
  openssl \
  php8.2

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
        ipython3

RUN pip install numpy
RUN pip install scipy
RUN pip install pandas

# Install node 16 and npm 8
RUN curl -s https://deb.nodesource.com/setup_16.x | bash && apt-get install nodejs -y

RUN apt-get update && apt-get install -y \
        php8.2-bcmath \
        php8.2-calendar \
        php8.2-curl \
        php8.2-exif \
        php8.2-gd \
        php8.2-iconv \
        php8.2-intl \
        php8.2-mbstring \
        php8.2-pdo \
        php8.2-pdo-pgsql \
        php8.2-pgsql \
        php8.2-tokenizer \
        php8.2-xml \
        php8.2-sockets \
        php8.2-zip

RUN curl -sS https://getcomposer.org/installer | php \
	&& mv composer.phar /usr/local/bin/composer \
	&& composer self-update

# Install Chrome
RUN apt-get install -y wget
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN apt install -y ./google-chrome-stable_current_amd64.deb


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
