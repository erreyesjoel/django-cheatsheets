# Nada mas instalar Django se genera un urls.pỳ
- Eso lo primero, es el archivo PRINCIPAL DE URLS
- Podemos definir las primeras rutas como de login o lo que sea (backend)
- Pero en ese urls.py principal, DEBEMOS INCLUIR LAS RUTAS (urls.py) de las demás Apps que tengamos
**Si no, no funcionarán, porque si no Django no sabe como enrutar las peticiones**

## Ejemplo con una app llamada juegos
1. urls.py (principal)
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
    path('juegos/', include('juegos.urls')), # incluimos las urls de la app juegos, juegos (app) urls (juegos/urls.py)
]
```
## Explicacion
- juegos/ y ahi metemos juegos.urls) -> Donde juegos (es el nombre de la app) y urls (es el fichero, urls.py)

2. Después el urls.py de nuestra app, en este caso juegos
- Donde hacemos las rutas "normal", el path y tal como siempre normal
```python
from django.urls import path # importamos path, para definir las rutas
from . import views # importamos las vistas del mismo directorio, osea de nuestra app (juegos)

urlpatterns = [
    path('', views.mostrarJuegos, name='mostrar_juegos'),
    path('eliminar/<int:juego_id>/', views.eliminarJuego, name='eliminar_juego'),
]
```

**ESO ES TODO**
