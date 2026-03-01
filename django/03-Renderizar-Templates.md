# Definicion de template que viene de una carpeta en la carpeta templates
### Aqui no usamos apps todavia
1. Creamos la carpeta Autenticacion en Templates 
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ ls templates/autenticacion/
login.html
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ ls
asgi.py  __init__.py  __pycache__  settings.py  templates  urls.py  views.py  wsgi.py
```
2. Dentro creamos un .html, login.html
```html
<h1>Login siuuu</h1>
```
3. En views.py definimos la funcion, y hacemos el render
```python
# def para renderizar el template de login

def login_template(request):
    return render(request, 'autenticacion/login.html')
```
4. Y por ultimo definimos la url en urls.py
```python
from django.contrib import admin
from django.urls import path
from . import views # import del views

urlpatterns = [
    path('', views.login_template, name='login_template') # template de login, sera la raiz del sitio
]
```

