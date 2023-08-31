# Используем базовый образ с Ubuntu и Python
FROM python:3.8-slim-buster

# Установка зависимостей
RUN apt-get update && apt-get install -y nginx postgresql postgresql-contrib

# Копирование кода проекта
COPY . /app
WORKDIR /app

# Установка зависимостей Python
RUN pip install -r requirements.txt

# Копирование конфига Nginx
COPY nginx.conf /etc/nginx/sites-available/default

# Запуск PostgreSQL и ожидание его запуска и инициализации
CMD service postgresql start && \
    sleep 5 && \
    su - postgres -c "psql -c 'CREATE DATABASE mydb;'" && \
    su - postgres -c "psql -c 'CREATE USER admin WITH PASSWORD '\''admin'\'';'" && \
    su - postgres -c "psql -c 'ALTER DATABASE mydb OWNER TO admin;'" && \
    sleep 5 && \
    python manage.py migrate && \
    sleep 5 && \
    gunicorn --bind 0.0.0.0:8000 myproject.wsgi:application