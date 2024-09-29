# Imagem oficial do PHP 8.2 com Apache como base
FROM php:8.2-apache

# Instalação de dependências necessárias
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libzip-dev \
    unzip \
    libonig-dev \
    git \
    libicu-dev \
    libxml2-dev \
    && rm -rf /var/lib/apt/lists/*

# Instale as extensões PHP necessárias
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install exif \
    && docker-php-ext-install pcntl \
    && docker-php-ext-install mysqli

# Expõe a porta 80 para o Apache
EXPOSE 80

# Instalação do Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Criação do projeto Laravel
WORKDIR /var/www/html
RUN composer create-project laravel/laravel:^8.0 project
RUN chmod 777 -R .

# Copia o modelo de arquivo de configuração do Apache
COPY ./apache.conf /etc/apache2/sites-available/000-default.conf.model
COPY ./apache.conf /etc/apache2/sites-available/000-default.conf
COPY ./chproj /bin/chproj
RUN chmod +x /bin/chproj

# Habilita o mod_rewrite do Apache
RUN a2enmod rewrite

ENTRYPOINT ["docker-php-entrypoint", "apache2-foreground"]
