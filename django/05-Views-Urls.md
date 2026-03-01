# views.py y urls.py

## ¿Qué es `views.py`?
Archivo donde se define la lógica del backend: funciones o clases que procesan peticiones, renderizan templates, devuelven JSON, gestionan formularios, autentican usuarios, etc.

Puntos clave:
- Se importan las librerías necesarias.
- Se definen funciones (`def`) o clases (CBV).
- Se renderizan templates con `render()`.
- Se redirige con `redirect()`.
- Se devuelven respuestas JSON si hace falta.
- Se pueden proteger vistas con `@login_required`.

---

## Ejemplo básico de `views.py`

```python
from django.shortcuts import render, redirect
from django.contrib.auth.decorators import login_required
from django.contrib.auth import authenticate, login, logout
from django.contrib import messages
from usuarios.models import ModeloUsuarioModificado

def login_template(request):
    if request.user.is_authenticated:
        return redirect('admin_dashboard_template')

    if request.method == 'POST':
        email = request.POST.get('email')
        password = request.POST.get('password')

        try:
            user_obj = ModeloUsuarioModificado.objects.get(email=email)
            user = authenticate(request, username=user_obj.username, password=password)

            if user is not None:
                if user.rol == ModeloUsuarioModificado.Roles.ADMIN:
                    login(request, user)
                    return redirect('admin_dashboard_template')
                messages.error(request, "No tienes permisos para acceder a esta zona.")
                return render(request, 'autenticacion/login.html')

            messages.error(request, "Credenciales inválidas.")
            return render(request, 'autenticacion/login.html')

        except ModeloUsuarioModificado.DoesNotExist:
            messages.error(request, "Credenciales inválidas.")
            return render(request, 'autenticacion/login.html')

    return render(request, 'autenticacion/login.html')

@login_required(login_url='/')
def admin_dashboard_template(request):
    return render(request, 'admin/dashboardAdmin.html', {
        'active_page': 'dashboard'
    })

@login_required(login_url='/')
def logout_view(request):
    logout(request)
    return redirect('login_template')
```

2. Archivo urls.py
- Es el archivo donde "definimos y unimos" la url con la def del views.py
- Importamos las views, views de las apps que tengamos, las mismas urls de otras apps
- Se definen las rutas con path()
- Ejemplo básico
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
