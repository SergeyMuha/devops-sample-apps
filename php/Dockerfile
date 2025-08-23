FROM php:8.2-apache

WORKDIR /var/www/html

# Copy application source
COPY index.php .
COPY config.dev .
COPY config.prod .

# Script to select config at container start
RUN echo '#!/bin/sh\n' \
    'if [ "$APP_ENV" = "prod" ]; then\n' \
    '  cp /var/www/html/config.prod /var/www/html/config\n' \
    'else\n' \
    '  cp /var/www/html/config.dev /var/www/html/config\n' \
    'fi\n' \
    'exec "$@"' > /entrypoint.sh \
    && chmod +x /entrypoint.sh

# Use our entrypoint, run apache
ENTRYPOINT ["/entrypoint.sh"]
CMD ["apache2-foreground"]
