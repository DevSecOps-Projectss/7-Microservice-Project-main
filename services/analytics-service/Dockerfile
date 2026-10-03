FROM php:8.3-cli-alpine
WORKDIR /app

# Runtime libraries + temporary build dependencies for PHP extensions.
RUN apk add --no-cache libpq libcurl && \
    apk add --no-cache --virtual .build-deps $PHPIZE_DEPS postgresql-dev curl-dev && \
    docker-php-ext-install pdo_pgsql curl && \
    apk del .build-deps && \
    addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --chown=appuser:appgroup router.php ./

USER appuser
EXPOSE 8087
CMD ["php", "-S", "0.0.0.0:8087", "router.php"]
