# Памятка: Реализация авторизации и регистрации в Django

## 📋 Задача
Реализовать регистрацию и авторизацию в проекте Django, чтобы пользователь мог попасть на главную страницу **только после входа**.

---

## ✅ Шаг 1: Подключение встроенной системы аутентификации Django

Убедись, что в `settings.py` в `INSTALLED_APPS` есть следующие приложения:

```python
INSTALLED_APPS = [
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

---

## ✅ Шаг 2: URL-маршруты для входа, выхода, регистрации и главной страницы

Добавь в `urls.py` маршруты:

```python
from django.urls import path
from django.contrib.auth import views as auth_views
from . import views

urlpatterns = [
    ...
    path('login/', auth_views.LoginView.as_view(), name='login'),
    path('logout/', auth_views.LogoutView.as_view(), name='logout'),
    path('register/', views.register, name='register'),
```

---

## ✅ Шаг 3: Представления (views)

### 📌 Регистрация

```python
from django.shortcuts import render, redirect
from django.contrib.auth.forms import UserCreationForm
from django.contrib.auth import login

def register(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            login(request, user)
            return redirect('home')
    else:
        form = UserCreationForm()
    return render(request, 'registration/register.html', {'form': form})
```

### 📌 Главная страница (только для авторизованных)

```python
from django.contrib.auth.decorators import login_required

@login_required
### Функция для отображения главной страницы
def home(request):
    return render(request, 'home.html')
```

---

## ✅ Шаг 4: Шаблоны (templates)

### 📌 `registration/login.html`

```html
<h2>Вход</h2>
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Войти</button>
</form>
```

### 📌 `registration/register.html`

```html
<h2>Регистрация</h2>
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Зарегистрироваться</button>
</form>
```

### 📌 Как можно сделать гиперссылку для выхода

```html
<h1>Добро пожаловать!</h1>
<a href="{% url 'logout' %}">Выйти</a>
```

---

## ✅ Шаг 5: Настройки в `settings.py`

Добавь или проверь:

```python
LOGOUT_REDIRECT_URL = 'login'
LOGIN_URL = 'login'
```

---

## ✅ Шаг 6: Проверка работы

1. Перейди на главную страницу без входа — тебя должно перекинуть на `/login/`.
2. Зарегистрируйся через `/register/` — ты попадёшь на главную.
3. После выхода — снова будет требоваться вход.

---

## ✅ Шаг 7: Миграции (если не делал ранее)

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🎉 Готово!

Теперь твой проект защищён авторизацией, и доступ к главной странице есть только у вошедших пользователей.

---

## ✅ Шаг 8: Если что-то пошло не так

### 🔐 Аутентификация

Убедись, что в `settings.py` явно указан backend аутентификации:

```python
AUTHENTICATION_BACKENDS = [ 
    'django.contrib.auth.backends.ModelBackend',  # стандартный backend
]
```

### 💾 Сессии

Проверь, что включён механизм сессий:

```python
SESSION_ENGINE = 'django.contrib.sessions.backends.db'
```

---

## ✅ Шаг 9: Проверка авторизации в шаблоне

Добавь этот код в шаблон, чтобы видеть, авторизован ли пользователь:

```django
{% if user.is_authenticated %}
    <p>Вы авторизованы как {{ user.username }}</p>
{% else %}
    <p>Вы не авторизованы</p>
{% endif %}
```