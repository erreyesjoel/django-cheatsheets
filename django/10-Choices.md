# Uso de Choices en Django y cómo mostrar el valor legible en templates

En Django, los campos con **choices** permiten definir un conjunto de valores internos (códigos) y sus nombres legibles (para mostrar en la web).
Esto es útil para mostrar información clara al usuario, mientras que el sistema usa valores técnicos para guardar en la base de datos.

---

## Ejemplo de campo `choices` en un modelo

Supongamos que tienes el modelo `Categoria` así:

```python
class Categoria(models.Model):
    ACCION = 'ACCION'
    AVENTURA = 'AVENTURA'
    DEPORTES = 'DEPORTES'
    # ...otros tipos...

    CATEGORIAS_CHOICES = [
        (ACCION, 'Acción'),
        (AVENTURA, 'Aventura'),
        (DEPORTES, 'Deportes'),
        # ...
    ]

    nombre_categoria = models.CharField(
        max_length=20,
        choices=CATEGORIAS_CHOICES,
        default=ACCION
    )
```

- El valor interno es lo que se guarda en la base de datos (por ejemplo, "ACCION").
- El valor legible es lo que se muestra al usuario (por ejemplo, "Acción").

Cómo mostrar el valor legible en el template
- En el template, si usas

```html
{{ juego.categoria.nombre_categoria }}
```
Verás el valor interno (por ejemplo, "ACCION").

Si quieres mostrar el nombre bonito definido en los choices, usa el método especial de Django:

```html
{{ juego.categoria.get_nombre_categoria_display }}
```

Esto mostrará "Acción" en vez de "ACCION".

Ejemplo completo en un template

```html
<!--
  El campo 'nombre_categoria' en el modelo Categoria es un choices.
  Por eso, para mostrar el nombre legible (por ejemplo, "Acción" en vez de "ACCION"),
  se usa {{ juego.categoria.get_nombre_categoria_display }}.
  Si usas {{ juego.categoria.nombre_categoria }}, verás el valor interno (en mayúsculas).
  Los choices permiten definir valores internos y nombres bonitos para mostrar en la web.
-->
<td>{{ juego.categoria.get_nombre_categoria_display }}</td>
```
Resumen
Usa choices en tus modelos para definir valores internos y legibles.
En los templates, usa get_FIELD_display para mostrar el nombre bonito.
Así tu web será más clara y profesional para los usuarios.

