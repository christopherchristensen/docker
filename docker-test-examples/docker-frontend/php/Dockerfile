FROM php:7.2.7-fpm-alpine3.7
RUN apk update; \
    apk upgrade;
RUN docker-php-ext-install mysqli

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

# The first command will grab the Composer installer as a PHAR (PHP Archive) file, while the second ensures that the installer is free from any errors or corruption. After executing these commands, you will have the latest build of Composer’s installer on your drive.

RUN php composer-setup.php --install-dir=bin --filename=composer

RUN php bin/composer install