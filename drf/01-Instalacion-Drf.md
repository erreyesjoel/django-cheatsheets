# Instalación de Django Rest Framework (drf)
### Que es drf?
- Framework oficial de Django para crear APIs REST. Permite devolver JSON, manejar autenticación, permisos, serialización de datos y construir endpoints de forma# Como podemos crear una api con drf
### IMPORTANTE, en settings.py, agregar en INSTALLED_APPS, 'rest_framework'
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'tablas',  # Aplicación personalizada para manejar tablas
    'rest_framework',  # Django REST Framework para crear APIs
]
```
1. Debemos tener instalado django rest framework
```bash
pip install djangorestframework 
```
- Añadir a requeriments | Actualizar el archivo de dependencias
```bash
pip freeze > requirements.txt
```
2. Crear una vista en 'views.py'
- views.py
```python
from rest_framework.decorators import api_view
from rest_framework.response import Response

@api_view(['GET'])
def ejemplo_get(request):
    data = {
        "mensaje": "¡Hola desde la API!",
        "paquete": {
            "nombre": "Tour a Machu Picchu",
            "precio": 199.99,
            "duracion": 3,
            "estado": "activo"
        }
    }
    return Response(data)
```
3. Despues, registrar la ruta en urls.py
- urls.py
```python
from django.contrib import admin
from django.urls import path
from tablas.views import ejemplo_get  # <--- importa la vista

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/ejemplo/', ejemplo_get, name='ejemplo_get'),  # <--- agrega la URL para la vista
]
```
4. Ahora arrancamos django
```bash
python3 manage.py runserver 8002
```
5. Y veremos nuestra api -> http://127.0.0.1:8002/api/ejemplo/
```json
HTTP 200 OK
Allow: GET, OPTIONS
Content-Type: application/json
Vary: Accept

{
    "mensaje": "¡Hola desde la API!",
    "paquete": {
        "nombre": "Tour a Machu Picchu",
        "precio": 199.99,
        "duracion": 3,
        "estado": "activo"
    }
}
```