# Instalacion backend (Ejemplo con un proyecto mio creado) GameHaven

### Antes de nada, dentro del repo de git, creamos una carpeta backend o como queramos y ahi metemos el entorno virtual venv, y vamos haciendo
```shell
joel-erreyes:~/docsjoel/proyectos personales/GameHaven$ mkdir backend
joel-erreyes:~/docsjoel/proyectos personales/GameHaven$ cd backend/
```

1. Crear el entorno virtual
```shell
python3 -m venv env
```
2. Activar entorno virtual (yo uso linux)
```shell
source env/bin/activate
```
3. Instalar django usando pip
```shell
pip install django
```
- Verificar version
```shell
django-admin --version
# Ejemplo tu versión: 5.2.3
```
4. Añadir django (lo que hemos instalado antes con pip) al requeriments.txt
```shell
pip freeze > requirements.txt
```
- Cada vez que instalemos un paquete pip, hacemos eso.
- Salida del terminal
```shell
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend$ pip install django
Collecting django
  Downloading django-5.2.6-py3-none-any.whl.metadata (4.1 kB)
Collecting asgiref>=3.8.1 (from django)
  Downloading asgiref-3.9.1-py3-none-any.whl.metadata (9.3 kB)
Collecting sqlparse>=0.3.1 (from django)
  Using cached sqlparse-0.5.3-py3-none-any.whl.metadata (3.9 kB)
Downloading django-5.2.6-py3-none-any.whl (8.3 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.3/8.3 MB 22.2 MB/s eta 0:00:00
Downloading asgiref-3.9.1-py3-none-any.whl (23 kB)
Using cached sqlparse-0.5.3-py3-none-any.whl (44 kB)
Installing collected packages: sqlparse, asgiref, django
Successfully installed asgiref-3.9.1 django-5.2.6 sqlparse-0.5.3
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend$ pip freeze > requirements.txt
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend$ cat requirements.txt
asgiref==3.9.1
Django==5.2.6
sqlparse==0.5.3
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend$
```

5. Crear proyecto django con su nombre, el que queramos
```shell
django-admin startproject backend
```
6. Levantar el servidor
```shell
python3 manage.py runserver
# Por defecto usa el puerto 8000
# Si está ocupado, puedes usar otro, por ejemplo:
python3 manage.py runserver 8002
```
# NOTA
- Si queremos eliminar el entorno virtual, porque lo hemos alojado donde no queriamos, hacemos

```shell
deactivate
rm -rf env
```
- Eso es todo.

# Que archivos o directorios poner en .gitignore antes de subir todo esto al repositorio de gitignore
1. El entorno virtual (venv o env)
2. Y mas cosas, este seria un git ignore general para el backend
```.gitignore
# Entorno virtual Python
env/

# Archivos de entorno y secretos
*.env

# Base de datos local de desarrollo
*.sqlite3

# Caché y compilados de Python
__pycache__/
*.py[cod]
*.pyo
*.pyd

# Migraciones compiladas
backend_django/*/migrations/*.pyc
backend_django/*/migrations/__pycache__/

# Archivos de logs
*.log

# Archivos de cobertura de tests
htmlcov/
.coverage
.tox/
.nox/
.pytest_cache/

# Archivos de IDE
.vscode/
.idea/
*.sublime-project
*.sublime-workspace
```


