# Dockerized Flask Web Application

## Описание

Этот проект демонстрирует автоматическое развертывание веб-приложения с использованием Docker и GitLab CI. Веб-приложение написано на Python с использованием Flask.

## Содержание

- [Описание](#Описание)
- [Содержание](#Содержание)
- [Установка](#Установка)
- [Docker-сборка](#Docker-сборка)
- [CI/CD с GitLab](#CICD-с-GitLab)
- [Отчет](#Отчет)
- [Контакты](#Контакты)

## Установка

1. Убедитесь, что у вас установлены Docker и GitLab Runner.
2. Клонируйте репозиторий:
   ```bash
   git clone https://gitlab.com/<your-namespace>/<your-repo>.git
   cd <your-repo>
   ```

## Docker-сборка

### Создание Docker-образа

1. Создайте файл `Dockerfile` со следующим содержимым:
   ```Dockerfile
   FROM python:3.9-slim

   RUN pip install flask

   COPY app.py /app.py

   WORKDIR /

   CMD ["python", "app.py"]
   ```

2. Создайте файл `app.py` со следующим содержимым:
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def hello_world():
       return 'Hello, World!'

   if __name__ == '__main__':
       app.run(host='0.0.0.0')
   ```

3. Постройте и запустите Docker-образ:
   ```bash
   docker build -t my-app .
   docker run -d -p 80:5000 my-app
   ```

4. Перейдите в браузер и откройте `http://localhost`, чтобы увидеть ваше приложение.

## CI/CD с GitLab

### Настройка GitLab CI

1. Создайте файл `.gitlab-ci.yml` со следующим содержимым:
   ```yaml
   stages:
     - build
     - test
     - deploy

   build:
     stage: build
     script:
       - docker build -t my-app .
       - docker tag my-app registry.gitlab.com/<your-namespace>/<your-repo>/my-app:latest
       - docker push registry.gitlab.com/<your-namespace>/<your-repo>/my-app:latest
     only:
       - main

   test:
     stage: test
     script:
       - echo "Run tests here"
     only:
       - main

   deploy:
     stage: deploy
     script:
       - docker pull registry.gitlab.com/<your-namespace>/<your-repo>/my-app:latest
       - docker run -d -p 80:5000 my-app
     only:
       - main
   ```

2. Закоммитьте и запушьте файлы в репозиторий GitLab:
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

### Настройка GitLab Runner

Убедитесь, что GitLab Runner настроен и зарегистрирован в вашем проекте. Для этого следуйте официальной документации GitLab.

## Отчет

Результаты выполнения задания представлены в отчете `report.pdf`, который включает в себя:
1. Описание задачи.
2. Код веб-приложения (`app.py`).
3. Содержимое Dockerfile.
4. Содержимое `.gitlab-ci.yml`.
5. Скриншоты успешного выполнения стадий в GitLab CI.
6. Скриншот открытой страницы веб-приложения в браузере.

