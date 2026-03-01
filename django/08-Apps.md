# ¿Qué son las Apps en Django?

Las **apps en Django** son módulos independientes que agrupan el código relacionado con una funcionalidad concreta. Cada app suele contener archivos como:

- `views.py` — lógica de las vistas
- `models.py` — modelos y tablas de la base de datos
- `admin.py` — configuración del panel de administración
- `apps.py` — configuración interna de la app
- `tests.py` — tests unitarios
- (Opcional) `urls.py` — rutas propias de la app
- (Opcional) carpeta `templates/` — plantillas HTML de esa app

Organizar el proyecto en apps permite mantener el código limpio, modular y fácil de mantener.

---

## Gestión de apps en Django

Django permite dividir el proyecto en múltiples apps, cada una encargada de una parte del sistema.

### Crear una app
```bash
python manage.py startapp juegos
```
- Esto genera una carpeta juegos/ con los archivos básicos (models.py, views.py, etc.).
- Además, ahi podemos crear un urls.py, para definir las urls de la app juegos en este caso
- juegos/urls.py
```python
from django.urls import path # importamos path, para definir las rutas
from . import views # importamos las vistas del mismo directorio, osea de nuestra app (juegos)

urlpatterns = [
    path('', views.mostrarJuegos, name='mostrar_juegos'),
]
```
- En el urls.py principal, se incluye el archivo de rutas de la app usando include()
```python
"""
URL configuration for backend project.

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/5.2/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path, include # include para incluir las urls de las apps
from . import views # import del views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.login_template, name='login_template'), # template de login, sera la raiz del sitio
    path('dashboard', views.admin_dashboard_template, name='admin_dashboard_template'), # template del dashboard de admin, http://127.0.0.1:8002/dashboard
    path("logout/", views.logout_view, name="logout"),
    path('juegos/', include('juegos.urls')), # incluimos las urls de la app juegos
]
```
## Resumen rápido
- Una app es un módulo independiente dentro de un proyecto Django.
- Se crea con startapp.
- Cada app puede tener sus propias vistas, modelos, templates y URLs.
- El proyecto principal incluye las rutas de cada app con include().