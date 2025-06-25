FROM php:8.3.0-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Install system dependencies FIRST
RUN apt-get update && apt-get install -y \
    curl \
    zip \
    unzip \
    git \
    libpq-dev \
    postgresql-server-dev-all \
    libjpeg-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libfreetype6-dev \
    libssl-dev \
    libzstd-dev

# Configure PHP extensions (só as que precisam de configuração específica)
RUN docker-php-ext-configure gd --with-freetype --with-jpeg

# Install PHP extensions (pdo_pgsql não precisa de configuração no PHP 8.3)
RUN docker-php-ext-install -j$(nproc) \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    gd \
    bcmath \
    opcache \
    mbstring \
    intl \
    exif \
    sockets \
    pcntl \
    zip

# Install Redis extension
RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/6.2.0.tar.gz \
    && tar xfz /tmp/redis.tar.gz -C /tmp \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext/redis \
    && mv /tmp/phpredis-6.2.0/* /usr/src/php/ext/redis \
    && rm -rf /tmp/phpredis-6.2.0 \
    && docker-php-ext-install redis

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_22.x | bash -
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

# Set environment variables
ENV APP_ENV=development
ENV APP_DEBUG=true

# Expose ports
EXPOSE 9000 80 443 5173

# Switch to non-root user
USER ${user}
