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

# Por ultimo, mostarlo en un .html
```html
{% include 'components/header/header.html' %}
{% load static %}
<link rel="stylesheet" href="{% static 'css/main.css' %}">

<div class="main-content">
  <div class="dashboard-page">

    <!-- Cabecera -->
    <h1 class="dashboard-title">Dashboard</h1>
    <p class="dashboard-subtitle">Resumen general de GameHaven</p>

    <!-- Tarjetas de estadísticas -->
    <div class="stats-grid">

      <!-- Usuarios -->
      <div class="stat-card card-usuarios">
        <div class="stat-card__icon">
          <svg viewBox="0 0 24 24"><circle cx="12" cy="7" r="4"/><path d="M5.5 20c0-3.59 2.91-6.5 6.5-6.5s6.5 2.91 6.5 6.5"/></svg>
        </div>
        <span class="stat-card__label">Usuarios</span>
        <span class="stat-card__value">{{ datos.total_usuarios }}</span>
        <a href="{% url 'listar_usuarios' %}" class="stat-card__link">
          Ver usuarios
          <svg viewBox="0 0 24 24"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
        </a>
        <a href="{% url 'listar_usuarios' %}" class="quick-action-btn secondary">
          <svg viewBox="0 0 24 24"><circle cx="12" cy="7" r="4"/><path d="M5.5 20c0-3.59 2.91-6.5 6.5-6.5s6.5 2.91 6.5 6.5"/></svg>
          Gestionar usuarios
        </a>
      </div>

      <!-- Juegos -->
      <div class="stat-card card-juegos">
        <div class="stat-card__icon">
          <svg viewBox="0 0 24 24"><rect x="2" y="6" width="20" height="14" rx="3"/><path d="M8 2h8M6 12h4M8 10v4M15 12h2"/></svg>
        </div>
        <span class="stat-card__label">Juegos</span>
        <span class="stat-card__value">{{ datos.total_juegos }}</span>
        <a href="{% url 'mostrar_juegos' %}" class="stat-card__link">
          Ver juegos
          <svg viewBox="0 0 24 24"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
        </a>
        <a href="{% url 'crear_juego' %}" class="quick-action-btn secondary">
          <svg viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></svg>
          Nuevo juego
        </a>
      </div>

      <!-- Plataformas -->
      <div class="stat-card card-plataformas">
        <div class="stat-card__icon">
          <svg viewBox="0 0 24 24"><rect x="3" y="14" width="18" height="4" rx="2"/><path d="M8 14V8a4 4 0 0 1 8 0v6"/></svg>
        </div>
        <span class="stat-card__label">Plataformas</span>
        <span class="stat-card__value">{{ datos.total_plataformas }}</span>
        <a href="{% url 'mostrar_plataformas' %}" class="stat-card__link">
          Ver plataformas
          <svg viewBox="0 0 24 24"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
        </a>
        <a href="{% url 'crear_plataforma' %}" class="quick-action-btn secondary">
          <svg viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></svg>
          Nueva plataforma
        </a>
      </div>

      <!-- Categorías -->
      <div class="stat-card card-categorias">
        <div class="stat-card__icon">
          <svg viewBox="0 0 24 24"><path d="M4 4h6v6H4zM14 4h6v6h-6zM4 14h6v6H4zM14 14h6v6h-6z"/></svg>
        </div>
        <span class="stat-card__label">Categorías</span>
        <span class="stat-card__value">{{ datos.total_categorias }}</span>
        <a href="#" class="stat-card__link">
          Ver categorías
          <svg viewBox="0 0 24 24"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
        </a>
        <a href="#" class="quick-action-btn secondary">
          <svg viewBox="0 0 24 24"><path d="M12 5v14M5 12h14"/></svg>
          Nueva categoría
        </a>
      </div>

    </div>
    <!-- /stats-grid -->

  </div>
</div>
```
