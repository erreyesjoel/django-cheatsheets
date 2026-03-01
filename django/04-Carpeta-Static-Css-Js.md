# Que es la carpeta static, para que sirve y donde se guarda
- La carpeta static es la que, por norma general, si usas templates html, donde se guardan los archivos css y js, o img (imagenes)
- Aunque tambien podemos guardar alguna carpeta ahi para subir imagenes mediante la aplicacion web, tanto con html o con apis (drf)
- Se puede guardar en la misma ubicacion en donde se encuentra la carpeta templates
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ ls backend/
asgi.py  __init__.py  __pycache__  settings.py  templates  urls.py  views.py  wsgi.py
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ ls backend/static/
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```
- Podemos crear dentro de static, css, js, ima (para imagenes)
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ ls backend/static/
css  img  js
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```
# PARA USAR SCSS EN DJANGO TEMPLATES

1. Ir a la carpeta static, y hacer
```shell
npm init -y
```
2. Instalar sass
```shell
npm install sass
```
3. Crear la carpeta scss -> static/scss

- Colocamos nuestros archivos .scss dentro de satic/scss
- **EJEMPLO** -> static/scss/estilos.scss

4. Ahora seria, si hago algo de css estilos, usar sass, los ficheros .scss en static/scss/, luego compilarlo a .css y que se guarde en static/css/

5. En .gitignore, metemos la carpeta node_modules
```.gitignore
# ignorar node modules de backend (django templates)
node_modules/
```
## Como trabajar ahora

1. Creamos un login.scss por ejemplo en static/scss

```scss
// Estilos de ejemplo para el login
$color-principal: #3498db;

body {
  background: #f4f4f4;
}

h1 {
  color: $color-principal;
  text-align: center;
  margin-top: 50px;
}
```

2. Lo compilamos el .scss a .css
```shell
npx sass scss/login.scss css/login.css
```
- **TODO ESO DESDE CARPETA STATIC, EN TERMINAL**
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend/static$
```

3. Se nos creará un login.css y un login.css.map en la carpeta static/css

4. Meter en .gitignore, para que los .map no se suban al repositorio
```.gitignore
# ignorar archivos de SASS compilados, porque se generan automaticamente y no es necesario versionarlos
*.css.map
```

5. Si no vemos los estilos, es porque en settings.py, no tendremos echa la configuracion de la carpeta static
```python
STATIC_URL = 'static/'  # URL base para servir archivos estáticos
# STATICFILES_DIRS indica a Django dónde buscar la carpeta 'static' personalizada fuera de las apps
STATICFILES_DIRS = [
    BASE_DIR / "backend" / "static",  # Ruta absoluta a la carpeta static
]
```
- **Y ESO ES TODO, deberia funcionar**

# ADICIONAL, podemos tener el main.scss

```scss
@use 'login';
```
- Compilamos el scss a css, dentro de static/
```shell
npx sass scss/main.scss css/main.css
```
- Y en los html, enlazamos al main.css
```html
<link rel="stylesheet" href="{% static 'css/main.css' %}">
```

# Tipografia

- Lo creamos, aqui solo le indicamos que selectores, clases, etc, tendran la tipografia
```scss
/* tipografia.scss */
$fuente-principal: 'Montserrat', sans-serif;

body, .login-container {
  font-family: $fuente-principal;
}
```

- En main.scss si hacemos el import de la tipografia
```scss
/* en sass todas las reglas @use y @import de scss deben ir antes de cualquier otra regla
incluyendo el @import url de css */
@use 'tipografia';
@use 'login';
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap');
```

- Compilamos de nuevo el main.scss
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django/backend/static$ npx sass scss/main.scss css/main.css
```
**SOLO CON COMPILAR EL _main.scss es suficiente, no hace falta compilar todos los .css**

## NOTA

- Los .scss, que empiezen por _ mejor, ejemplo -> _header.scss
- Pero al compilarlo a css, lo compilamos sin la _, osea quedaria -> header.css


# JAVA script en carpeta static
- Simplemente creamos el archivo js en static/js
- Y en el html, lo enlazamos
```html
{% load static %}
<script src="{% static 'js/login.js' %}"></script>
```







