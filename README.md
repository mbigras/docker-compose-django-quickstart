# docker compose-django-quickstart

> Implement the Compose and Django Docker tutorial.

# Summary

This document follows and refactors the [Quickstart: Compose and Django](https://docs.docker.com/samples/django/) Docker tutorial to practice clarity in technical documentation.

# Getting started

To run a Django app that connects to a Postgres database with Docker Compose, run the following procedure.

1. Install and configure Docker Compose.

   See the [Install Docker Compose Docker](https://docs.docker.com/compose/install/) page.

   > :bulb: **Note:** This document was last updated on 2022-09-05 and ran with the following Docker Compose version:
   >```
   >$ docker compose version
   >Docker Compose version v2.10.2
   >```

1. Create your project directory.

   ```
   mkdir compose-project
   ```

1. Navigate to your project directory.

   ```
   cd compose-project
   ```

1. Create and edit your `Dockerfile` file.

   Your `Dockerfile` should contain the following text:

   ```
   FROM python:3
   ENV PYTHONDONTWRITEBYTECODE=1
   ENV PYTHONUNBUFFERED=1
   WORKDIR /code
   COPY requirements.txt /code/
   RUN pip install -r requirements.txt
   COPY . /code/
   ```

1. Create and edit your `requirements.txt` file.

   Your `requirements.txt` file should contain the following text:

   ```
   Django>=3.0,<4.0
   psycopg2>=2.8
   ```

1. Create and edit your `docker compose.yml` file.

   Your `docker compose.yml` file should contain the following text:

   ```
   version: "3.9"
   
   services:
     db:
       image: postgres
       volumes:
         - ./data/db:/var/lib/postgresql/data
       environment:
         - POSTGRES_DB=postgres
         - POSTGRES_USER=postgres
         - POSTGRES_PASSWORD=postgres
     web:
       build: .
       command: python manage.py runserver 0.0.0.0:8000
       volumes:
         - .:/code
       ports:
         - "8000:8000"
       environment:
         - POSTGRES_NAME=postgres
         - POSTGRES_USER=postgres
         - POSTGRES_PASSWORD=postgres
       depends_on:
         - db
   ```

1. Create your Django project—created in the `djangoproject` directory.

   ```
   sudo docker compose run web django-admin startproject djangoproject .
   ```

1. Check the files and directories in your project directory.

   ```
   ls -l
   ```

   Your terminal output should look like the following:

   ```
   $ ls -l
   # ...
   ```

1. Ownership.

1. To connect your Django app to your database, edit your `djangoproject/settings.py` file.

   Your `djangoproject/settings.py` file should contain the following text:

   ```
   # ...
   import os
   # ...
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.postgresql',
           'NAME': os.environ.get('POSTGRES_NAME'),
           'USER': os.environ.get('POSTGRES_USER'),
           'PASSWORD': os.environ.get('POSTGRES_PASSWORD'),
           'HOST': 'db',
           'PORT': 5432,
       }
   }
   ```

1. Start your Django app and database.

   ```
   docker compose up
   ```

   Your terminal output should look like the following:

   ```
   $ docker compose up
   # ...
   ```

1. In your web browser, navigate to http://localhost:8000.

   Your web browser should look like the following:

   <img width="1076" alt="Screen Shot 2022-09-04 at 11 02 41 PM" src="https://user-images.githubusercontent.com/5590632/188371428-95c66f36-d860-415b-93ab-a67e9dbbfb28.png">

1. In a new terminal window, navigate back to your project directory.

1. Stop and remove your app and database.

   ```
   docker compose down
   ```
