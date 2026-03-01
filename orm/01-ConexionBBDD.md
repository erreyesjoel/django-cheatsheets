# Seguridad: SECRET_KEY en Django

## ¿Por qué es importante la SECRET_KEY en Django?

La `SECRET_KEY` es fundamental en cualquier proyecto Django, sin importar la base de datos utilizada (PostgreSQL, MySQL, SQLite, etc). Esta clave:

- Se utiliza para firmar cookies, tokens y datos sensibles generados por Django.
- Protege contra ataques como falsificación de solicitudes (CSRF), manipulación de sesiones y otros riesgos de seguridad.
- Si un atacante obtiene tu `SECRET_KEY`, podría generar sesiones válidas, modificar cookies o descifrar información protegida.

**Nunca compartas ni subas tu `SECRET_KEY` a repositorios públicos.**

Siempre almacénala en el archivo `.env` o en variables de entorno seguras, y nunca directamente en el código fuente.

### ¿Cómo generar y guardar la SECRET_KEY?

1. Genera una nueva clave secreta:
    ```bash
    python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
    ```
2. Añádela a tu archivo `.env` (en la misma carpeta que `settings.py` o `manage.py`):
    ```env
    SECRET_KEY=tu_clave_generada
    ```

# Conexion a base de datos postgres en Django
1. Crear un .env, en donde esta el settings.py
```.env
DB_NAME=basededatos
DB_USER=usuario_basededatos
DB_PASSWORD=password
DB_HOST=localhost
DB_PORT=5432 o 5433
```
2. Instalar el python-dotenv
- Permite cargar variables del `.env` en Django.
```shell
pip install python-dotenv
```
3. Meterlo al requeriments.txt para actualizar dependencias
```shell
pip freeze > requirements.txt
```
4. Cargar las variables en settings.py
```python
import os
from dotenv import load_dotenv

load_dotenv()

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('DB_NAME'),
        'USER': os.getenv('DB_USER'),
        'PASSWORD': os.getenv('DB_PASSWORD'),
        'HOST': os.getenv('DB_HOST'),
        'PORT': os.getenv('DB_PORT'),
    }
}
```
5. Ahora hacer python manage.py migrate, para probar la conexion con la base de datos

- Si no funciona, es que nos falta instalar el driver de PostgreSQL
```shell
pip install psycopg2-binary
```
- Actualizar dependencias
```shell
pip freeze > requirements.txt
```

6. python manage.py migrate
- salida terminal

```shell
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ ls
backend  db.sqlite3  manage.py
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```
- En el schema public, en pgadmin, deberiamos ver esas tablas creadas!
- Por defecto django usará siempre el schema public

# Conexión a base de datos MySQL/MariaDB en Django
1. Activar el entorno virtual
Si no está activado:
```bash
source env/bin/activate
```
2. Instalar dependencias necesarias
- Librería para gestionar variables de entorno:
```bash
pip install django-environ
```
- Driver para conectar Django con MySQL/MariaDB:
```bash
pip install pymysql
```
- Actualizar dependencias:
```bash
pip freeze > requirements.txt
```
3. Crear archivo .env
- Debe estar en la misma carpeta donde está manage.py o donde cargues las variables.
- Ejemplo:
- .env para Django + MySQL/MariaDB
```env
SECRET_KEY=tu_clave_generada
DEBUG=True
```
- settings.py
```python
# Configuración de la base de datos
DB_ENGINE=django.db.backends.mysql
DB_NAME=easytripdb
DB_USER=easytripuser
DB_PASSWORD=easytrip123
DB_HOST=127.0.0.1
DB_PORT=3310
```
4. Configurar settings.py para usar .env y PyMySQL
```python
from pathlib import Path
import environ
import os
import pymysql

pymysql.install_as_MySQLdb()

BASE_DIR = Path(__file__).resolve().parent.parent

# Inicializar django-environ
env = environ.Env(
    DEBUG=(bool, False)
)
environ.Env.read_env(os.path.join(BASE_DIR, '.env'))

# Seguridad
SECRET_KEY = env('SECRET_KEY')
DEBUG = env('DEBUG')

ALLOWED_HOSTS = []

# Base de datos
DATABASES = {
    'default': {
        'ENGINE': env('DB_ENGINE'),
        'NAME': env('DB_NAME'),
        'USER': env('DB_USER'),
        'PASSWORD': env('DB_PASSWORD'),
        'HOST': env('DB_HOST'),
        'PORT': env('DB_PORT'),
    }
}
```
5. Generar una nueva SECRET_KEY (opcional) si no la tenemos
...existing code...
6. Crear o actualizar requirements.txt
```bash
pip freeze > requirements.txt
```
- Para instalar dependencias en otro entorno:
```bash
pip install -r requirements.txt
```
7. Probar la conexión ejecutando migraciones
- El .env debe estar en la misma ubicación que manage.py.
```bash
python manage.py migrate
```
- Salida esperada:
```bash
Applying contenttypes.0001_initial... OK
Applying auth.0001_initial... OK
Applying admin.0001_initial... OK
...
Applying sessions.0001_initial... OK
```
8. Nota importante si usas Docker
- Asegúrate de que los contenedores estén levantados:
```bash
docker compose up -d
```
O
```bash
docker-compose up -d
```
## Resumen rápido
- .env → credenciales y configuración segura
- django-environ → cargar variables
- PyMySQL → driver para MySQL/MariaDB
- migrate → prueba de conexión
- Docker debe estar levantado si la DB está en contenedor