FROM php:7.4-apache

# mysqli extension enable
RUN docker-php-ext-install mysqli

# Set proper permissions
RUN chown -R www-data:www-data /var/www/html
RUN chmod -R 755 /var/www/html

# Create uploads directory and set permissions
RUN mkdir -p /var/www/html/uploads/member_photos
RUN chown -R www-data:www-data /var/www/html/uploads
RUN chmod -R 755 /var/www/html/uploads
