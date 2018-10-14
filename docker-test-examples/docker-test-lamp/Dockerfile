# Set the working directory to /app
WORKDIR /var/www/html

# Make port 80 available to the world outside this container
EXPOSE 80

# Run app.py when the container launches
CMD ["php", "index.php"]

FROM php:7.2-apache
COPY src/ /var/www/html/