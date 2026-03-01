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
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend$ ls
backend_django  env  requirements.txt
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend$ pip freeze > requirements.txt
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
```shell
pip freeze > requirements.txt

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
![alt text](<resultado al crear el server.png>)