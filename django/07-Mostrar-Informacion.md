# Como mostrar informacion en django templates (cosas del backend mostrar)
- tengo un views.py e, app juegos -> juegos/views.py
- donde devolvemos todos los juegos y renderizamos el template .html
```python
from django.shortcuts import render
from juegos.models import Juego # importamos el modelo
from plataformas.models import Plataforma # importamos el modelo

# Create your views here.
# renderizar juegos.html y funcion para devolver los juegos
def mostrarJuegos(request):
    juegos = Juego.objects.all() # obtenemos todos los juegos
    plataformas = Plataforma.objects.all() # obtenemos todas las plataformas
    return render(request, 'admin/juegos.html', {'juegos': juegos, 'plataformas': plataformas})
```

- objects.all - > porque queremos TODOS los datos, sin condicion alguna
- y el html, no tiene estilos, pero para ver como se muestra, nos puede servir
```python
{% include 'components/header/header.html' %}

<h1>Juegos</h1>
<table>
    <thead>
        <tr>
            <th>ID</th>
            <th>Título</th>
            <th>Descripción</th>
            <th>Categoría</th>
            <th>Plataformas</th>
            <th>Foto</th>
        </tr>
    </thead>
    <tbody>
        {% for juego in juegos %}
        <tr>
            <td>{{ juego.id }}</td>
            <td>{{ juego.titulo }}</td>
            <td>{{ juego.descripcion }}</td>
            <td>{{ juego.categoria.get_nombre_categoria_display }}</td>
            <td>
                {% for jp in juego.juegos_plataformas_set.all %}
                    {{ jp.plataforma.get_tipo_display }}{% if not forloop.last %}, {% endif %}
                {% empty %}
                    Sin plataformas
                {% endfor %}
            </td>
            <td>
                {% with foto=juego.fotos_juegos_set.first %}
                    {% if foto %}
                        {% if foto.url %}
                            <img src="{{ foto.url }}" alt="Foto de {{ juego.titulo }}" width="80"><br>
                            <small>{{ foto.url }}</small>
                        {% else %}
                            Sin foto (no hay URL)
                        {% endif %}
                    {% else %}
                        Sin foto (no hay registro)
                    {% endif %}
                {% endwith %}
            </td>
        </tr>
        {% endfor %}
    </tbody>
</table>
```


