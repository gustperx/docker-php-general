FROM php:8.1.6-fpm

# Get argument defined in docker-compose file
ARG user
ARG uid

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=UTC

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \ 
    g++ \ 
    make \
    ca-certificates \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    vim \
    nano \
    && curl -sL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get install -y yarn \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install exif \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install gd \
    && docker-php-ext-install intl \
    && docker-php-source delete

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Get latest Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 $user
RUN useradd -u 1000 -ms /bin/bash -g $user $user

# Copy existing application directory contents
# COPY . /var/www

# Copy existing application directory permissions
COPY --chown=$user:$user . /var/www

# Set working directory
WORKDIR /var/www

USER $user