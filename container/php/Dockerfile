ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm-alpine

LABEL maintainer="guillaume.chauveau@appus.fr"

ARG APCU_VERSION
ARG AMQP_VERSION
ARG COMPOSER_VERSION

RUN apk add --no-cache acl file gettext git mysql-client
RUN set -eux; \
	apk add --no-cache --virtual .build-deps $PHPIZE_DEPS icu-dev libzip-dev mysql-dev zlib-dev rabbitmq-c-dev; \
	apk add --no-cache bash sed grep vim; \
	docker-php-ext-configure zip --with-libzip; \
	docker-php-ext-install -j$(nproc) intl pdo_mysql zip bcmath sockets; \
	pecl install apcu-${APCU_VERSION}; \
	pecl install amqp-${AMQP_VERSION}; \
	pecl clear-cache;
RUN	docker-php-ext-enable opcache apcu amqp; \
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --no-cache --virtual .api-phpexts-rundeps $runDeps; \
	apk del .build-deps

ENV MEMCACHED_DEPS zlib-dev libmemcached-dev cyrus-sasl-dev
RUN apk add --no-cache --update libmemcached-libs zlib
RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && apk add --no-cache --update --virtual .memcached-deps $MEMCACHED_DEPS \
    && pecl install memcached \
    && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/20_memcached.ini \
    && rm -rf /usr/share/php7 \
    && rm -rf /tmp/* \
    && apk del .memcached-deps .phpize-deps

RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer --version=$COMPOSER_VERSION \
    && chmod +x /usr/local/bin/composer

# COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint
# RUN chmod +x /usr/local/bin/docker-entrypoint

COPY config/php.ini /usr/local/etc/php/php.ini

CMD ["php-fpm"]

EXPOSE 9000