# Templates en Django
- Por defecto están en la carpeta templates, donde esta el wsgy.py, settings.py etc... Creamos la carpeta 'templates'

```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ ls
asgi.py  __init__.py  __pycache__  settings.py  urls.py  wsgi.py
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ ls
asgi.py  __init__.py  __pycache__  settings.py  templates  urls.py  wsgi.py
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$
```
- Ahora podemos crear un .html ahi dentro
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ ls templates/
template_principal_ejemplo.html
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$
```
## NOTA
- No hace falta tener apps, para templates o apis, solo son una forma de organizar la aplicacion

- Como ya tenemos el .html creado en templates, en la misma ubicacion donde esta la carpeta /templates
creamos el archivo views.py
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ ls
asgi.py  __init__.py  __pycache__  settings.py  templates  urls.py  views.py  wsgi.py
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$
```
# IMPORTANTE
- En settings.py, debemos definir el directorio en donde estan nuestras plantillas o templates html
```python
from pathlib import Path
import os # para poder trabajar con rutas de archivos y directorios de forma compatible entre sistemas operativos

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        # en dirs, definimos el directorio donde se encuentran nuestras plantillas HTML
        'DIRS': [os.path.join(BASE_DIR, 'backend', 'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

- el html

```html
<h1>Ejemplo ruta</h1>
```
- la view en views.py
```python
from django.shortcuts import render # importante para poder renderizar los html

def ejemplo_template(request):
    return render(request, 'template_principal_ejemplo.html')
```

- el urls.py

```python
from django.contrib import admin
from django.urls import path
from . import views # import del views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('ejemplo/', views.ejemplo_template, name='ejemplo_template'), # nuestro template
]




