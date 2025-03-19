# Django Quick Start Guide

## 📌 Что такое Django?
Django — это мощный веб-фреймворк для Python, который позволяет быстро создавать надежные и масштабируемые веб-приложения. Он упрощает разработку за счет встроенных инструментов, таких как ORM (система работы с базами данных), маршрутизация, аутентификация и система шаблонов.

📖 [Официальная документация Django](https://docs.djangoproject.com/en/stable/)

### Когда использовать Django?
Django идеально подходит для:
- Разработки полноценных веб-приложений
- Создания новостных порталов и блогов
- CRM, ERP-систем
- API-серверов
- Сайтов с динамическим контентом

---

## 🚀 Установка Django и создание проекта

### 1️⃣ Установить Django
```bash
pip install django
```

### 2️⃣ Создать новый проект
```bash
django-admin startproject myproject
```

### 3️⃣ Запустить сервер разработки
```bash
cd myproject
python manage.py runserver
```
Открыть в браузере [http://127.0.0.1:8000/](http://127.0.0.1:8000/) и убедиться, что Django работает.

---

## 📄 Создание первой страницы

### 1️⃣ Создать новое приложение
```bash
python manage.py startapp myapp
```

### 2️⃣ Добавить `myapp` в `INSTALLED_APPS` в `settings.py`:
```python
INSTALLED_APPS = [
    ...,
    'myapp',
]
```

### 3️⃣ Определить представление (view) в `views.py`
```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Привет, Django!")
```

### 4️⃣ Настроить маршруты в `urls.py`
```python
from django.urls import path
from myapp.views import home

urlpatterns = [
    path('', home),
]
```

### 5️⃣ Перезапустить сервер и проверить страницу
```bash
python manage.py runserver
```

Перейти в браузере по адресу [http://127.0.0.1:8000/](http://127.0.0.1:8000/).

---

## 🎨 Работа с шаблонами и динамическим контентом

### 1️⃣ Создать папку `templates` в `myapp` и файл `index.html`

```html
<html>
    <head> 
    <title>Приложение ДЖАНГО</title>
    </head>
    <body>
        <h1>Это ДЖАНГО приложение</h1>
        <a>Какой-то приветсвенный текст!</a> 
    </body>
</html>
```

```html
<html>
<head><title>Факты о числах</title></head>
<body>
    <h1>Факт о числе {{ number }}</h1>
</body>
</html> 
```

### 2️⃣ Обновить `views.py`
```python
from django.shortcuts import render
import random

def home(request):
    number = random.randint(1, 100)
    return render(request, 'index.html', {'number': number})
```

### 3️⃣ Проверить работу динамической страницы в браузере
Перейти по [http://127.0.0.1:8000/](http://127.0.0.1:8000/) и убедиться, что выводится случайное число.

---

### 4️⃣ Добавить на страницу рандомный факт о числе
Например, с помощью API - http://numbersapi.com/ 

## 🎯 Заключение
Теперь у вас есть базовое понимание, как создать Django-проект, добавить в него приложение, создать простую страницу и использовать шаблоны. 🚀

🔗 Дополнительные ресурсы:
- [Официальная документация](https://docs.djangoproject.com/en/stable/)
- [Django на GitHub](https://github.com/django/django)
