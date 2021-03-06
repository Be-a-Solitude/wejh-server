FROM php:7.4-fpm

# Arguments defined in docker-compose.yml
ARG user
ARG uid
ARG DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Clear cache
RUN apt-get autoremove --purge -y && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

RUN pecl install xdebug-3.0.2 \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.mode=debug\n\
xdebug.start_with_request=1\n\
xdebug.client_host=host.docker.internal\n\
xdebug.discover_client_host=1" > /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

# Set working directory
WORKDIR /var/www

USER $user
