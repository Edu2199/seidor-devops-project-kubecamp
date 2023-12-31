# # Etapa de construcción
# FROM php:8.2-fpm AS builder

# RUN apt-get update && apt-get install -y \
#     libssl-dev \
#     libzip-dev \
#     unzip \
#     git

# RUN pecl install mongodb \
#     && docker-php-ext-enable mongodb \
#     && docker-php-ext-install zip

# # Instala Composer
# RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# WORKDIR /var/www/html

# COPY . .

# RUN composer install --no-dev

# # Segunda etapa para la imagen final
# FROM php:8.2-fpm-alpine

# # Instala dependencias adicionales
# RUN apk add --no-cache libpq \
#     autoconf \
#     g++ \
#     make \
#     unzip \
#     git

# WORKDIR /var/www/html

# COPY --from=builder /var/www/html/vendor ./vendor

# COPY . .

# CMD ["composer", "install", "--no-dev", "&&", "php-fpm"]

 
# Etapa de construcción
FROM php:8.2-fpm AS builder

RUN apt-get update && apt-get install -y \
    libssl-dev \
    libzip-dev \
    unzip \
    git \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb \
    && docker-php-ext-install zip

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
COPY composer.json /var/www/html/composer.json
RUN composer install

WORKDIR /var/www/html

COPY . .


# Etapa final
FROM php:8.2-fpm-alpine

#RUN addgroup -g 1000 appuser && \
#    adduser -D -u 1000 -G appuser appuser

RUN apk add libpq \
  autoconf \
  g++ \
  make \
  unzip \
  git

RUN pecl install mongodb \
  && docker-php-ext-enable mongodb

# Cambiar el propietario de los archivos y directorios
#RUN chown -R appuser:appuser /var/www/html

#USER appuser

WORKDIR /var/www/html

COPY --from=builder /var/www/html/vendor ./vendor

COPY . .

CMD ["php-fpm"]