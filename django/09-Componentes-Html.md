# Como reutilizar un componente con y en Django
- Tener el .html o el template en si en la carpeta templates de django
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ ls 
asgi.py  __init__.py  __pycache__  settings.py  static  templates  urls.py  views.py  wsgi.py
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ ls templates/components/header/
header.html
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend$ 
```

- Luego incluirlo donde queramos, el html que queramos, con {% include %}
```html
{% include "components/header/header.html" %}
```

- NOTA: Tambien podemos pasar variables al incluir
```html
{% include "components/header/header.html" with titulo="Mi título" %}
```
**MUY IMPORTANTE**
- SI es un componente para reutilizar, es decir un html para reutilizar por asi decirlo
- Debemos quitar las etiquetas <html>, <head> y <body>
- Solo dejar el contenido del componente, por ejemplo
```html
<header>
    <h1>GameHaven</h1>
    <!-- Aquí tu menú, logo, etc. -->
</header>
```
- Para asi no duplicar la estructura html dentro de otro template
- Asi he hecho el include en el dashboard por ejemplo
```html
<!-- include, para incluir el header, donde queramos -->
{% include 'components/header/header.html' %}
<h1>admin</h1>
```

