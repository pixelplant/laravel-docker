FROM php:7.4-alpine

# Install dev dependencies
RUN apk add --no-cache --virtual .build-deps \
    $PHPIZE_DEPS \
    curl-dev \
    imagemagick-dev \
    libtool \
    libxml2-dev \
    postgresql-dev \
    sqlite-dev

# Install production dependencies
RUN apk add --no-cache \
    bash \
    curl \
    freetype-dev \
    g++ \
    gcc \
    git \
    icu-dev \
    icu-libs \
    imagemagick \
    libc-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libzip-dev \
    make \
    mysql-client \
    nodejs \
    nodejs-npm \
    oniguruma-dev \
    yarn \
    openssh-client \
    postgresql-libs \
    rsync \
    zlib-dev \
	postgresql \
	postgresql-contrib

# Install PECL and PEAR extensions
RUN pecl install \
    imagick

# Enable PECL and PEAR extensions
RUN docker-php-ext-enable \
    imagick 
	
# Install Selenium
RUN apk add openjdk11
RUN apk add --no-cache \
	chromium chromium-chromedriver
RUN wget https://selenium-release.storage.googleapis.com/3.141/selenium-server-standalone-3.141.59.jar \
	&& mv -f selenium-server-standalone-3.141.59.jar /usr/local/bin/selenium-server-standalone.jar \
	&& chown root:root /usr/local/bin/selenium-server-standalone.jar \
	&& chmod 0755 /usr/local/bin/selenium-server-standalone.jar

	
# Lighthouse
RUN apk add --no-cache \
    chromium \
    xvfb \
    && npm i -g lighthouse

# Configure php extensions
RUN docker-php-ext-configure gd --with-freetype --with-jpeg

# Install php extensions
RUN docker-php-ext-install \
    bcmath \
    calendar \
    curl \
    exif \
    gd \
    iconv \
    intl \
    mbstring \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    pgsql \
    pdo_sqlite \
    pcntl \
    tokenizer \
    xml \
	sockets \
    zip
	
# Install wkhtmltopdf
RUN apk add --update --no-cache \
    libgcc libstdc++ libx11 glib libxrender libxext libintl \
    ttf-dejavu ttf-droid ttf-freefont ttf-liberation ttf-ubuntu-font-family
	
COPY --from=madnight/alpine-wkhtmltopdf-builder:0.12.5-alpine3.10-606718795 \
    /bin/wkhtmltopdf /bin/wkhtmltopdf

# Install composer
ENV COMPOSER_HOME /composer
ENV PATH ./vendor/bin:/composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# Install PHP_CodeSniffer
RUN composer global require "squizlabs/php_codesniffer=*"

# Cleanup dev dependencies
RUN apk del -f .build-deps \
	&& mkdir -p /run/postgresql/ \
	&& chown -R postgres:postgres /run/postgresql/

# Setup working directory
WORKDIR /var/www

USER postgres

RUN mkdir -p /var/lib/postgresql/data &&\
	chmod 0700 /var/lib/postgresql/data &&\
	initdb /var/lib/postgresql/data &&\
	echo "host all  all    127.0.0.1/32  md5" >> /var/lib/postgresql/data/pg_hba.conf &&\
    echo "listen_addresses='*'" >> /var/lib/postgresql/data/postgresql.conf &&\
    pg_ctl start -D /var/lib/postgresql/data &&\
    psql --command "CREATE USER homestead WITH SUPERUSER PASSWORD 'secret';" &&\
    createdb -O homestead neho_test
	
USER root
