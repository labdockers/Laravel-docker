FROM php:8.3.0-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Install system dependencies FIRST
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Install PHP extensions (pdo_pgsql não precisa de configuração no PHP 8.3)
RUN docker-php-ext-install -j$(nproc) \
    pdo_mysql \
    gd \
    bcmath \
    mbstring \
    exif \
    sockets \
    pcntl

# Install Redis extension if use phpredis in development, case use predis not need
#RUN pecl install redis \
#    && docker-php-ext-enable redis

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_20.x | bash -
RUN apt-get update && apt-get install -y nodejs
RUN curl -qL https://www.npmjs.com/install.sh | sh

# Copy Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user
RUN useradd -G www-data,root -u ${uid} -d /home/${user} ${user}
RUN mkdir -p /home/${user}/.composer && \
    chown -R ${user}:${user} /home/${user}


# Set working directory
WORKDIR /var/www

# Create log directories
RUN mkdir -p /var/log/nginx \
    && mkdir -p /var/log/php-fpm \
    && mkdir -p /var/log/supervisor

# Expose ports
EXPOSE 9000 80 443 5173

# Switch to non-root user
USER ${user}
