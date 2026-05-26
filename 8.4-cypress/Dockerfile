FROM ubuntu:22.04
ARG DEBIAN_FRONTEND=noninteractive

# Single layer for all apt installations
RUN apt-get update && \
    apt-get install -y --no-install-recommends software-properties-common ca-certificates gnupg && \
    add-apt-repository ppa:ondrej/php && \
    apt update && apt upgrade -y && \
    apt-get install -y --no-install-recommends \
        openssl openssh-client php8.4 curl wget git zip unzip \
        rsync acl cron openvpn \
        libgtk2.0-0 libgtk-3-0 libgbm1 libnotify4 libnss3 \
        libxss1 libasound2 libxtst6 xauth xvfb \
        python3 python3-pip php8.4-imagick \
        php8.4-bcmath php8.4-calendar php8.4-curl php8.4-exif \
        php8.4-gd php8.4-iconv php8.4-intl php8.4-mbstring php8.4-pdo \
        php8.4-pdo-pgsql php8.4-pgsql php8.4-tokenizer \
        php8.4-xml php8.4-sockets php8.4-zip \
        php-codesniffer ghostscript && \
    # Install Node.js \
    curl -s https://deb.nodesource.com/setup_20.x | bash && \
    apt-get install -y --no-install-recommends nodejs && \
    # Install Composer \
    curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    composer self-update && \
    # Install pnpm \
    # Enable corepack (comes with Node.js) \
    corepack enable && \
    corepack prepare pnpm@latest --activate && \
    # Install Python packages \
    pip install --no-cache-dir numpy==1.25.0 scipy pandas && \
    # Install postgres \
    sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' && \
    curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg && \
    apt-get update && \
    apt-get install -y postgresql-16 postgresql-client-16 && \
    # Install Chrome \
    wget -q https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
    apt-get install -y ./google-chrome-stable_current_amd64.deb && \
    rm google-chrome-stable_current_amd64.deb && \
    # Install Chrome headless shell \
    wget -q https://storage.googleapis.com/chrome-for-testing-public/142.0.7444.6/linux64/chrome-headless-shell-linux64.zip && \
    unzip -q chrome-headless-shell-linux64.zip && \
    rm chrome-headless-shell-linux64.zip && \
    # Clean up \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cache

WORKDIR /var/www

USER postgres

RUN pg_ctlcluster 16 main start && \
    pg_ctlcluster 16 main status && \
    psql --command "CREATE USER homestead WITH SUPERUSER PASSWORD 'secret';" && \
    createdb -O homestead neho_test && \
    echo "host all  all    127.0.0.1/32  md5" >> /etc/postgresql/16/main/pg_hba.conf && \
    echo "listen_addresses='*'" >> /etc/postgresql/16/main/postgresql.conf

USER root
