FROM php:8.2-apache

WORKDIR /var/www/html

# Copy application source
COPY index.php .
COPY config.dev .
COPY config.prod .

# Enable Apache mod_rewrite
RUN a2enmod rewrite

# Add .htaccess with rewrite rules
RUN echo 'RewriteEngine On\n' \
         'RewriteCond %{REQUEST_FILENAME} !-f\n' \
         'RewriteCond %{REQUEST_FILENAME} !-d\n' \
         'RewriteRule ^.*$ index.php [QSA,L]' > /var/www/html/.htaccess

# Allow .htaccess overrides in Apache
RUN sed -i 's/AllowOverride None/AllowOverride All/' /etc/apache2/apache2.conf

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
