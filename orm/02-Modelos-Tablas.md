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
**En los models, la columna id, como clave primaria se "hace sola"**
# Definir relaciones
### TABLA CON RELACION (1:N)
- Primero creamos la N
- Tabla categoria en este caso
```python
from django.db import models

# Create your models here.
# clase para la tabla categorias

class Categoria(models.Model):
    class Categorias(models.TextChoices):
        DEPORTES = 'DEPORTES', 'Deportes'
        LUCHAS = 'LUCHAS', 'Luchas'
        CARRERAS = 'CARRERAS', 'Carreras'
        ACCION = 'ACCION', 'Acción'
        AVENTURAS = 'AVENTURAS', 'Aventuras'
        RPG = 'RPG', 'Rol (RPG)'
        ESTRATEGIA = 'ESTRATEGIA', 'Estrategia'
        SIMULACION = 'SIMULACION', 'Simulación'
        PUZZLE = 'PUZZLE', 'Puzzle'
        TERROR = 'TERROR', 'Terror'
        SHOOTER = 'SHOOTER', 'Shooter'
        PLATAFORMAS = 'PLATAFORMAS', 'Plataformas'
        MUSICAL = 'MUSICAL', 'Musical'
        PARTY = 'PARTY', 'Party'
        SANDBOX = 'SANDBOX', 'Sandbox'
        MMO = 'MMO', 'MMO'
        EDUCATIVO = 'EDUCATIVO', 'Educativo'
        OTROS = 'OTROS', 'Otros'

    nombre_categoria = models.CharField(
        max_length=30,
        choices=Categorias.choices,
        unique=True
    )

    class Meta:
        db_table = 'categorias' # nombre de la tabla en la base de datos, lo elegimos nosotros con el class meta
```
- Ahora la tabla 1, juegos en este caso
```python
from django.db import models
# importamos la app categorias con el from, y con el import la clase Categoria
from categorias.models import Categoria

# Create your models here.
# clase para la tabla juegos
class Juego(models.Model):
    PEGI_CHOICES = [
        (3, 'PEGI 3'),
        (7, 'PEGI 7'),
        (12, 'PEGI 12'),
        (16, 'PEGI 16'),
        (18, 'PEGI 18'),
    ]
    titulo = models.CharField(max_length=50, unique=True)
    descripcion = models.TextField(max_length=100)
    precio = models.DecimalField(max_digits=6, decimal_places=2)
    stock = models.IntegerField()
    fecha_lanzamiento = models.DateField()
    pegi = models.IntegerField(choices=PEGI_CHOICES)
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE) # Categoria es la clase que importamos de categorias.models, el class Categoria

    class Meta:
        db_table = 'juegos' # nombre de la tabla en la base de datos, lo elegimos nosotros con el class meta
```

# TABLA N:M
- Juegos plataformas, hay que crear tabla pivote
- Puedo hacerlo en el models.py de la app que quiera, juegos, o plataformas
- Lo haré en el models.py de app juegos, importando el modelo plataformas
```python
from django.db import models
# importamos la app categorias con el from, y con el import la clase Categoria
from categorias.models import Categoria
# importamos la app plataformas con el from, y con el import la clase Plataforma
from plataformas.models import Plataforma

# Create your models here.
# clase para la tabla juegos
class Juego(models.Model):
    PEGI_CHOICES = [
        (3, 'PEGI 3'),
        (7, 'PEGI 7'),
        (12, 'PEGI 12'),
        (16, 'PEGI 16'),
        (18, 'PEGI 18'),
    ]
    titulo = models.CharField(max_length=50, unique=True)
    descripcion = models.TextField(max_length=100)
    precio = models.DecimalField(max_digits=6, decimal_places=2)
    stock = models.IntegerField()
    fecha_lanzamiento = models.DateField()
    pegi = models.IntegerField(choices=PEGI_CHOICES)
    categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE) # Categoria es la clase que importamos de categorias.models, el class Categoria

    class Meta:
        db_table = 'juegos' # nombre de la tabla en la base de datos, lo elegimos nosotros con el class meta

# clase para tabla pivote, relacion muchos a muchos N:M entre juegos y plataformas
class Juegos_Plataformas(models.Model):
    juego = models.ForeignKey(Juego, on_delete=models.CASCADE) # Juego es la clase que importamos de juegos.models, el class Juego
    plataforma = models.ForeignKey(Plataforma, on_delete=models.CASCADE) # Plataforma es la clase que importamos de plataformas.models, el class Plataforma

    class Meta:
        db_table = 'juegos_plataformas' # nombre de la tabla en la base de datos, lo elegimos nosotros con el class meta
        unique_together = ('juego', 'plataforma') # para que no se repitan los pares juego-plataforma en la tabla pivote, es decir, que un juego no pueda estar dos veces en la misma plataforma
```
- Ahora solo queda aplicar las migraciones
```shell
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ ls
backend  categorias  db.sqlite3  juegos  manage.py  plataformas  usuarios
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py makemigrations
Migrations for 'juegos':
  juegos/migrations/0002_juegos_plataformas.py
    + Create model Juegos_Plataformas
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, categorias, contenttypes, juegos, plataformas, sessions, usuarios
Running migrations:
  Applying juegos.0002_juegos_plataformas... OK
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```