# Разработка Django-приложения "Список фильмов"

### Цели занятия:
1. Освоить работу с виртуальным окружением и Poetry.
2. Создать Django-проект и настроить базу данных.
3. Разработать модель данных для хранения информации о фильмах.
4. Реализовать форму для добавления фильмов через веб-интерфейс.
5. Проверить работоспособность механизма сохранения данных в БД.

## 1. Разворачивание виртуального окружения и создание Django-приложения 
### Шаги:
1. Установи Poetry, если он не установлен:
   ```bash
   pip install poetry
   ```
2. Создай новый проект и активируй окружение:
   ```bash
   poetry new movie_project
   cd movie_project
   poetry shell
   ```
3. Установи Django:
   ```bash
   poetry add django
   ```
4. Создай Django-проект:
   ```bash
   django-admin startproject config .
   ```
5. Запусти сервер и убедись, что проект работает:
   ```bash
   python manage.py runserver
   ```

## 2. Настройка БД и создание модели данных 
### Создание приложения
1. Создай приложение `movies` внутри Django-проекта:
   ```bash
   python manage.py startapp movies
   ```
2. Добавь приложение в `INSTALLED_APPS` в `settings.py`:
   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'movies',  # Добавляем наше приложение
   ]
   ```

### Шаблон кода models.py:
```python
from django.db import models

class Movie(models.Model):
    title = models.CharField(max_length=255)
    description = models.TextField()
    release_year = models.IntegerField()
    genre = models.CharField(max_length=100)

    def __str__(self):
        return self.title
```
### Шаги:
1. Выполни миграции:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```
2. Создай суперпользователя для работы с админкой:
   ```bash
   python manage.py createsuperuser
   ```

## 3. Реализация формы добавления фильмов
### Шаблон кода forms.py
```python
from django import forms
from .models import Movie

class MovieForm(forms.ModelForm):
    class Meta:
        model = Movie
        fields = ['title', 'description', 'release_year', 'genre']
```
#### views.py
```python
from django.shortcuts import render, redirect
from .models import Movie
from .forms import MovieForm

def add_movie(request):
    if request.method == "POST":
        form = MovieForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect("movie_list")
    else:
        form = MovieForm()
    return render(request, "add_movie.html", {"form": form})
```
#### urls.py
Маршруты в `config/urls.py`


В файле `urls.py` проекта ты подключаешь маршруты для всех приложений, используя функцию `include()`. Это позволяет сохранять проект в модульной структуре и избегать перегрузки маршрутов.

Пример файла `config/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('movies/', include('movies.urls')),  # Подключаем маршруты из приложения 'movies'
]
```
Маршруты в `movies/urls.py`
В приложении movies ты создаешь отдельный файл маршрутов urls.py, в котором определяешь маршруты, касающиеся именно этого приложения.

Пример файла movies/urls.py:

```python
from django.urls import path
from .views import add_movie

urlpatterns = [
    path('add/', add_movie, name='add_movie'),  # Маршрут для добавления фильма
]
```

#### add_movie.html
```html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Добавить фильм</button>
</form>
```

## 4. Работа с админкой Джанго
В файле `admin.py` ты можешь зарегистрировать свои модели, чтобы они отображались в админке Django.

Пример регистрации модели Movie:

```python
from django.contrib import admin
from .models import Movie

admin.site.register(Movie)
```

Убедись, что ты создал суперпользователя.
После этого, зайди в админку, используя http://localhost:8000/admin/, и введи логин и пароль суперпользователя.

В админке ты сможешь создавать, редактировать и удалять записи в базе данных, используя интерфейс Django.
