# Django Notes

## Table of Contents
- [Models](./notes/model.md)
- [URLs](./notes/urls.md)
- [Views](./notes/views.md)
- [Forms](./notes/forms.md)
- [Admin](./notes/admin.md)

# Django

## How to start project ?

**Django Overview** :

* Django is a mature and rapid development framework.
* Avoid comparing it to JavaScript frameworks; closer comparisons can be made to Laravel or Ruby on Rails.

**Virtual Environment Setup** :

* Virtual environments isolate dependencies and prevent conflicts.

1. *Commands for creating virtual environments*:

* Use `python -m venv <env_name>` or tools like `venv`, `pip`, and `pipx`.
* Activation:
  * On Mac/Linux: `source <env_name>/bin/activate`
  * On Windows: `source <env_name>\Scripts\activate`

2. *Installation Process* :

* Install Django using `pip install django`.
* Use `pip` commands within the virtual environment to ensure proper package management.

### `requirements.txt` File

This helps ensure your project’s dependencies are easily shared and installed across environments.

1. *Creating:*

   - Generate: `pip freeze > requirements.txt`
2. *Updating:*

   - After installing new packages, update: `pip freeze > requirements.txt`
3. *Installing:*

   - To install all dependencies from `requirements.txt`: `pip install -r requirements.txt`

**Advantages of Django** :

* Scalable and efficient even on minimal system resources.
* Popular among corporations for reducing infrastructure costs.
* Secure by default, protecting against common vulnerabilities.

---

### Flow of request

![screenshot](Images/Screenshot%202024-12-24%20134542.jpg)

---

### Levels of Django

![screenshot](Images/Screenshot%202024-12-24%20154859.jpg)

---

## Starting a New Django Project

1. Create project: `django-admin startproject <project_name>`
2. Navigate: `cd <project_name>`
3. Run server: `python manage.py runserver`

---

## Running an App

1. Create app: `python manage.py startapp <app_name>`
2. Register in `settings.py` under `INSTALLED_APPS`

---

# Django Essentials

---

### 1. `urls.py`

- Maps URLs to their respective views.
- Contains `urlpatterns` list, which routes user requests.
- Use `path()` or `re_path()` for routing:
  ```python
  from django.urls import path
  from . import views

  urlpatterns = [
      path('', views.home, name='home'),
  ]
  ```
- **Dynamic URL Patterns**
  - Capture values from URLs using angle brackets (<>).
    - Example: `path('post/<int:id>/', views.post_detail, name='post_detail')`
    - <int:id> captures an integer value from the URL and passes it to the post_detail view as a keyword argument.
- Important: Include app-level URLs in project-level `urls.py`:
  ```python
  from django.urls import include
  urlpatterns = [
      path('app/', include('app.urls')),
  ]
  ```

---

### 2. `views.py`

- Contains Python functions/classes that handle requests and return responses.
- Common types of views:
  - **Function-based views (FBV):**
    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, World!")
    ```
  - **Class-based views (CBV):**
    ```python
    from django.views import View
    from django.http import HttpResponse

    class HomeView(View):
        def get(self, request):
            return HttpResponse("Hello, World!")
    ```
- Can render templates using `render()`:
  ```python
  from django.shortcuts import render

  def home(request):
      return render(request, 'home.html')
  ```

---

### 3. Templates

- HTML files used to render data dynamically.
- Stored in the `templates/` folder within apps or at the project level.
- Use Django Template Language (DTL) for dynamic content:
  ```html
  <h1>Welcome, {{ user.name }}</h1>
  ```
- Configure template directory in `settings.py`:
  ```python
  TEMPLATES = [
      {
          'BACKEND': 'django.template.backends.django.DjangoTemplates',
          'DIRS': [BASE_DIR / 'templates'],
      },
  ]
  ```

---

### 4. Static Files

- Includes CSS, JS, images, etc.
- Stored in the `static/` folder within apps or at the project level.
- Define static files in `settings.py`:
  ```python
  STATIC_URL = '/static/'
  STATICFILES_DIRS = [BASE_DIR / 'static']
  ```
- Use `{% static %}` tag in templates:
  ```html
  <link rel="stylesheet" href="{% static 'css/style.css' %}">
  ```

---

### 5. Important Settings

- **Base Directory**:
  ```python
  import os
  BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  ```
- **Debug Mode** (for development):
  ```python
  DEBUG = True
  ```
- **Allowed Hosts** (for production):
  ```python
  ALLOWED_HOSTS = ['localhost', 'yourdomain.com']
  ```
- **Database Configuration**:
  ```python
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.sqlite3',
          'NAME': BASE_DIR / 'db.sqlite3',
      }
  }
  ```
- **Static Files & Media Files**:
  ```python
  STATIC_URL = '/static/'
  MEDIA_URL = '/media/'
  MEDIA_ROOT = BASE_DIR / 'media'
  ```


# **Jinja2**

## Overview:
- Jinja2 is a templating engine used in web frameworks like Django and Flask.
- It enables dynamic generation of HTML with template tags, filters, and expressions.

## Key Features:

### **Template Tags:**
- `{% ... %}`: For logic (loops, conditions).
- **Example:**  
  ```django
  {% for item in items %} {{ item }} {% endfor %}
  ```

### **Template Variables:**
- `{{ ... }}`: For inserting dynamic data.
- **Example:**  
  ```django
  {{ user.name }}
  ```

### **Filters:**
- Modify data within templates.
- **Example:**  
  ```django
  {{ name|capitalize }}
  ```

### **Control Structures:**
- **Conditional Statements:**  
  ```django
  {% if user.is_authenticated %} 
      Welcome, {{ user.name }} 
  {% else %} 
      Please log in 
  {% endif %}
  ```
- **Loops:**  
  ```django
  {% for post in posts %} 
      {{ post.title }} 
  {% endfor %}
  ```

### **Includes and Extends:**
- **Reusable Templates:**  
  ```django
  {% include 'header.html' %}
  ```
- **Template Inheritance:**  
  ```django
  {% extends 'base.html' %}
  {% block content %}
      Your content here.
  {% endblock %}
  ```

---

# **Django Apps**

### **What are Django Apps?**
- Modular components of a Django project, designed to handle specific functionalities (e.g., blog, user authentication).
- Each app can be reused across projects.

### **Structure of an App:**
1. **models.py**: Define database models.  
2. **views.py**: Handle request/response logic.  
3. **urls.py**: Map URLs to views.  
4. **admin.py**: Configure admin panel for the app.  
5. **apps.py**: App configuration (metadata).  
6. **migrations/**: Database migration files.

### **Creating an App:**
```bash
python manage.py startapp app_name
```

### **Registering an App:**
- Add app name in `INSTALLED_APPS` inside `settings.py`:  
  ```python
  INSTALLED_APPS = [
      'app_name',
  ]
  ```

### **Key Commands for Apps:**
- **Make Migrations:**  
  - Generates migration files based on changes made to your models (models.py).
  ```bash
  python manage.py makemigrations
  ```
- **Run Migrations:**  
  - Applies the migration files to the database.
  - Updates the database schema to reflect the changes defined in the migrations.
  ```bash
  python manage.py migrate
  ```
- **When to Use?**
  - After Modifying Models:
    - Run makemigrations to create migration files.
    - Then, run migrate to apply these changes.
  - On a Fresh Setup:
    - If you clone a project with existing migrations, only migrate is needed to set up the database.

---
## **Adding Tailwind with Reload and Managing Superuser in Django (Windows, Python venv)**


### **1. Adding Tailwind in Windows (Python venv)**

#### Steps to Add Tailwind:
1. **Install Tailwind Package**:  
   ```bash
   pip install django-tailwind
   ```

2. **Create a Tailwind App**:  
   ```bash
   python manage.py tailwind init <tailwindapp_name/none=default>
   ```

3. **Install Node.js and npm**:  
   - [Download Node.js](https://nodejs.org) and ensure it's added to your system PATH.  
   - Verify Installation:  
     ```bash
     node -v
     npm -v
     ```

4. **Install Tailwind Dependencies**:  
   ```bash
   python manage.py tailwind install
   ```

5. **Configure `settings.py`**: 
   Add `tailwind` and the app name (`<tailwindapp_name>`) to `INSTALLED_APPS`.

6. **Start Tailwind in Watch Mode**:  
   ```bash
   python manage.py tailwind start
   ```

---

### **2. Adding Tailwind Reload**

#### Steps to Enable Browser Reload with Tailwind:
1. **Install Django Browser Reload**:  
   ```bash
   pip install django-browser-reload
   ```

2. **Configure `settings.py`**:  
   Add `'django_browser_reload'` to `INSTALLED_APPS`.
   Add `'django_browser_reload.middleware.BrowserReloadMiddleware'` to `MIDDLEWARE`.

3. **Update `urls.py`**:  
   Add the browser reload URL pattern:  
   ```python
   urlpatterns += [path("__reload__/", include("django_browser_reload.urls"))]
   ```

4. **Update HTML Templates**:  
   Add the reload script inside the `<head>` tag:  
   ```html
   <script src="http://127.0.0.1:8000/__reload__/reload.js"></script>
   ```

5. **Run the Server**:  
   ```bash
   python manage.py runserver
   ```

---

## **Superuser Management**

#### Create a Superuser:  
- Command:  
  ```bash
  python manage.py createsuperuser
  ```  
- Enter details (username, email, and password).

#### Change Superuser Password:  
- Command:  
  ```bash
  python manage.py changepassword <username>
  ```  
- Follow the prompts to reset the password.

---

