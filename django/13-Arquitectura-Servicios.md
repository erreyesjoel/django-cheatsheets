# Capa de servicios en Django

## ¿Qué es un service?
Explicación breve…

## ¿Por qué usar services.py?
- Separar lógica de negocio de las vistas
- Mantener vistas limpias
- Reutilizar lógica en varios lugares
- Facilitar testing

## Ejemplo real
- App llamada dashboard, donde definiremos diferentes funciones para devolver usuarios, juegos, etc, lo que tengamos en la base de datos (cantidad o lo que sea)
- services.py
```python
from usuarios.models import ModeloUsuarioModificado
from juegos.models import Juego
from plataformas.models import Plataforma
from categorias.models import Categoria

def mostrar_datos_dashboard():
    return {
        'total_usuarios': ModeloUsuarioModificado.objects.count(),
        'total_juegos': Juego.objects.count(),
        'total_plataformas': Plataforma.objects.count(),
        'total_categorias': Categoria.objects.count(),
    }
```
- En el views.py de la misma app, importakmos el services con su funcion
```python
from django.shortcuts import render
from django.contrib.auth.decorators import login_required
from .services import mostrar_datos_dashboard

@login_required(login_url='/')
def admin_dashboard(request):
    datos = mostrar_datos_dashboard()
    return render(request, 'admin/dashboardAdmin.html', {
        'active_page': 'dashboard',
        'datos': datos
    })
```
## Cuándo crear un service
- Cuando una vista empieza a tener demasiada lógica, o creamos que lo tendra, para no hacer multiples funciones que devuelvan cosas en la misma views.py, y para no tener millones de urls o endpoints innecesarios
- Cuando varias vistas necesitan la misma operación
- Cuando quieres preparar el proyecto para crecer
