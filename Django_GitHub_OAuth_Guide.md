# 🐍 Django + GitHub OAuth2: Инструкция

Эта инструкция поможет реализовать авторизацию через GitHub в Django-проекте с использованием `social-auth-app-django`.

---

## 📦 Что понадобится

- Django-проект
- GitHub-аккаунт
- Библиотека `social-auth-app-django`

---

## ✅ Шаг 1: Установка зависимостей

```bash
pip install social-auth-app-django
```

---

## ✅ Шаг 2: Настройка Django (`settings.py`)

### Добавь в `INSTALLED_APPS`:

```python
INSTALLED_APPS += [
    'social_django',
]
```

### Добавь аутентификационные бэкенды:

```python
AUTHENTICATION_BACKENDS = (
    'social_core.backends.github.GithubOAuth2',
    'django.contrib.auth.backends.ModelBackend',
)
```

### OAuth ключи:

```python
SOCIAL_AUTH_GITHUB_KEY = 'ВАШ_CLIENT_ID'
SOCIAL_AUTH_GITHUB_SECRET = 'ВАШ_CLIENT_SECRET'
```

### URL перенаправления:

```python
LOGIN_REDIRECT_URL = '/'
LOGOUT_REDIRECT_URL = '/'
```

### Middleware:

```python
MIDDLEWARE += [
    'social_django.middleware.SocialAuthExceptionMiddleware',
]
```

### Подключи context_processors:

```python
'context_processors': [
    ...
    'social_django.context_processors.backends',
    'social_django.context_processors.login_redirect',
],
```

---

## ✅ Шаг 3: Миграции

```bash
python manage.py migrate
```

---

## ✅ Шаг 4: Регистрация OAuth-приложения на GitHub

1. Перейди на [https://github.com/settings/developers](https://github.com/settings/developers)
2. Нажмите **"New OAuth App"**
3. Укажите:
   - **Application name**: My Django App
   - **Homepage URL**: `http://localhost:8000`
   - **Authorization callback URL**: `http://localhost:8000/complete/github/`

4. Скопируйте `Client ID` и `Client Secret` и вставьте их в `settings.py`

---

## ✅ Шаг 5: Подключение URL-ов (`urls.py`)

```python
from django.urls import include, path

urlpatterns = [
    path('', include('django.contrib.auth.urls')),
    path('', include('social_django.urls', namespace='social')),
]
```

---

## ✅ Шаг 6: Кнопка входа через GitHub

Добавьте в шаблон (`login.html` или другой):

```html
<a href="{% url 'social:begin' 'github' %}">Войти через GitHub</a>
```

---

## ✅ Шаг 7: Запуск сервера

```bash
python manage.py runserver
```

Перейди в браузер, и проверь, как работает GitHub-авторизация.

---

## 🎁 Бонус: Получение данных из GitHub

```python
def profile(request):
    user = request.user
    github_data = user.social_auth.get(provider='github')
    access_token = github_data.extra_data['access_token']
    username = github_data.extra_data.get('login', 'no username')

    return render(request, 'profile.html', {
        'username': username,
        'token': access_token,
    })
```

---

## 🧠 Полезные ссылки

- Документация: https://python-social-auth.readthedocs.io/
- GitHub OAuth apps: https://github.com/settings/developers

---

Удачи в разработке! 🚀