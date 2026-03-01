# Gestion de la tabla auth_user de Django
- Que és la tabla auth_user de Django?
**Es la tabla por defecto de Django, tabla por defecto de usuarios, donde tenemos la columna, first_name, last_name, username, last_login etc...**
- Podemos utilizarla, ya que como se ve tiene campos ya predefinidos, muy útiles...

## Como podemos "gestionar" esa tabla, como por ejemplo, añadir columnas, o modificar campos?
- Pues lo más recomendable, es extender el modelo de usuario usando un modelo personalizado
- No es modificar la tabla auth_user ni sus campos, porque podemos romper compatibilidad con Django y sus apps
** COMO LO HACEMOS? **

1. Creamos una nueva app, por ejemplo usuarios, por llamarla de algun modo, pero podemos llamarla como queramos
```shell
python manage.py startapp usuarios
```
- Eso lo hacemos en el directorio donde esté el manage.py
- Nos creará un directorio usuarios, donde habrá views.py, models.py..., donde en models.py, podemos añadir columnas, relaciones, etc...
- Salida del terminal del comando, lo que nos ha creado en la app usuarios
```shell
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ ls usuarios/
admin.py  apps.py  __init__.py  migrations  models.py  tests.py  views.py
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ 
```

2. Crear un modelo personalizado extendiendo AbstractUser
- usuarios/models.py
```python
from django.db import models
from django.contrib.auth.models import AbstractUser

# Create your models here.
# enum, choices, choices es como enums en otros lenguajes
# AbstractUser es el modelo por defecto de django para usuarios
class ModeloUsuarioModificado(AbstractUser):
    class Roles(models.TextChoices):
        ADMIN = 'ADMIN', 'Administrador'
        USER = 'USER', 'Usuario'

    rol = models.CharField(
        max_length=10,
        choices=Roles.choices,
        default=Roles.USER
    )
```
3. En settings.py, metemos nuestra app que creamos, startapp usuarios en INSTALLED_APPS
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'usuarios',  # nuestra app de usuarios (python manage.py startapp usuarios)
]
```
4. En settings.py metemos AUTH_USER_MODEL, metemos nuestro modelo de models.py de la app usuarios 
```python
AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

AUTH_USER_MODEL = 'usuarios.ModeloUsuarioModificado'
```
5. Ahora hay que correr las migraciones
```shell
python manage.py makemigrations
python manage.py migrate
```
### NOTA
- Si corrimos las migraciones antes de usuarios, nos dará error el migrate
- Para solucionarlo
1. borramos la base de datos, la volvemos a crear vacia con el mismo nombre
2. Eliminamos las migraciones previas (borrar todo dentro de las carpetas migrations dentro de nuestras apps (excepto __init__.py)
3. Solo dejamos el archivo __init__.py en cada carpeta
**POR ESTO, ES MEJOR, CREEAR PRIMERO EL MODELO PERSONALIZADO DE USUARIO, ANTES DE CORRER LAS MIGRACIONES**

- Y deberia de funcionar, salida del terminal
```shell
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py makemigrations
Migrations for 'usuarios':
  usuarios/migrations/0001_initial.py
    + Create model ModeloUsuarioModificado
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, usuarios
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0001_initial... OK
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
  Applying usuarios.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying sessions.0001_initial... OK
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ 
```
- Ahora ya tendremos las tablas de nuevo, y tabla usuarios, tendrá la columna rol que le hemos puesto
- Por cierto si queremos ponerle un nombre especifico a la tabla, en models.py, petemos un class_meta
```python
    class Meta:
        db_table = 'usuarios'  # nombre de la tabla en la base de datos
```
- Corremos migraciones
```shell
python manage.py makemigrations usuarios
python manage.py migrate
```
- Salida del terminal exitosa
```shell
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py makemigrations usuarios
Migrations for 'usuarios':
  usuarios/migrations/0002_alter_modelousuariomodificado_options_and_more.py
    ~ Change Meta options on modelousuariomodificado
    ~ Rename table for modelousuariomodificado to usuarios
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, usuarios
Running migrations:
  Applying usuarios.0002_alter_modelousuariomodificado_options_and_more... OK
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ 
```
# NOTA
- La carpeta migrations/ DEBE estar en el repo, para que todos los desarrolladores y servidores tengan la misma estructura de base de datos
- Solo debemos ignorar los archivos temporales como __pycache__, pero no los archivos .py de migraciones

