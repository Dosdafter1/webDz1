# Устанавливаем базовый образ PHP
FROM php:8.2-fpm

# Установка утилиты unzip
RUN apt-get update && apt-get install -y unzip git mc

# Тут же необходимо установить все необхоимые пакеты

# Устанавливаем рабочую директорию
WORKDIR /var/www/html
# Копирование и установка зависимостей PHP
COPY ./src /var/www/html


# Копирование и установка Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN composer install

# Установка Nginx
RUN apt-get update && apt-get install -y nginx

# Копирование конфигурационных файлов Nginx
COPY ./nginx.conf /etc/nginx/nginx.conf

# Открываем порт для доступа к приложению
EXPOSE 80

# Установка supervisord
RUN apt-get install -y supervisor

# Копирование конфигурационного файла supervisord для воркеров очереди
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Запуск сервисов контейнера
CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
