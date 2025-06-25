FROM php:8.3.0-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd sockets

# Install Redis extension - refer to latest releases: https://github.com/phpredis/phpredis/tags
RUN apt-get update && apt-get install -y \
    libzstd-dev && \
    curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/6.2.0.tar.gz \
    && tar xfz /tmp/redis.tar.gz -C /tmp \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext/redis \
    && mv /tmp/phpredis-6.2.0/* /usr/src/php/ext/redis \
    && rm -rf /tmp/phpredis-6.2.0 \
    && docker-php-ext-install redis

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Para alterar o caminho da versão LTS do node acessar essa página
# https://github.com/nodesource/distributions#debmanual
RUN curl -sL https://deb.nodesource.com/setup_22.x | bash -
RUN apt-get update && apt-get install -y nodejs

# Instalar a versão mais recente do NPM
RUN curl -qL https://www.npmjs.com/install.sh | sh
RUN apt-get update

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user


# Quando entrar no container, esse vai ser pasta principal que ele vai entrar
WORKDIR /var/www
# Aqui vai remover a pasta html
RUN rm -rf /var/www/html


# Liberar porta 9000 para iniciar o php-fpm server
EXPOSE 9000
EXPOSE 443
EXPOSE 80
EXPOSE 5173

USER $user
