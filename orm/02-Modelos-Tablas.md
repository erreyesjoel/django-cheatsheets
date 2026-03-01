# Modelos, Tablas, Relaciones
1. Supongamos que tenemos una app plataformas creada
```bash
python manage.py startapp plataformas
```
- Automaticamente nos creará el models.py
- En models.py es donde definimos las tablas, relaciones (fk's), etc
- models.py
- NOTA: choices es como un enum
```python
from django.db import models

# Create your models here.
# clase para la tabla plataformas

class Plataforma(models.Model):
    class Tipos(models.TextChoices):
        PC = 'PC', 'PC'
        PLAYSTATION = 'PLAYSTATION', 'PlayStation'
        XBOX = 'XBOX', 'Xbox'
        NINTENDO = 'NINTENDO', 'Nintendo'

    tipo = models.CharField(
        max_length=20,
        choices=Tipos.choices,
    )

    class Meta:
        db_table = 'plataformas' # nombre de la tabla en la base de datos, lo elegimos nosotros con el class meta
```

- Por ultimo, donde está el manage.py, hacemos los comandos para aplicar las migraciones
```shell
python manage.py makemigrations
```
```shell
python manage.py migrate
```
- Salida del terminal -> python manage.py makemigrations
```shell
Migrations for 'plataformas':
  plataformas/migrations/0001_initial.py
    + Create model Plataforma
```
- Salida del terminal -> python manage.py migrate
```shell
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, plataformas, sessions, usuarios
Running migrations:
  Applying plataformas.0001_initial... OK
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```
# NOTA
1. En los models, la columna id, como clave primaria se "hace sola"