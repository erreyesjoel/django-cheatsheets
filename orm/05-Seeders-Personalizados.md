 # Seeders Personalizados en Django
 1. En donde están todas nuestras apps, creamos una app llamada 'seeders'
 - en backend/backend_django
 ```bash
joel-erreyes:~/docsjoel/proyectos personales/GameHaven$ ls backend/backend_django/
backend  categorias  db.sqlite3  juegos  manage.py  plataformas  usuarios
joel-erreyes:~/docsjoel/proyectos personales/GameHaven$
```
2. Dentro de la carpeta, creamos la app
```bash
python manage.py startapp seeders
```
- Dentro de seeders/ creamos la carpeta management/commands
```bash
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ ls seeders/management/
commands
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```
3. Y dentro de management/commands
- El seeder maestro, llamado seed.py
```python
from django.core.management.base import BaseCommand
from django.core.management import call_command

class Command(BaseCommand):
    help = "Ejecuta todos los seeders del proyecto"

    def handle(self, *args, **kwargs):
        self.stdout.write(self.style.WARNING("Iniciando seeders..."))

        # Orden de ejecucion de todos los seeders
        call_command("seed_categorias")
        call_command("seed_plataformas")
        call_command("seed_usuarios")
        call_command("seed_juegos")

        self.stdout.write(self.style.SUCCESS("Seeders ejecutados correctamente"))
```
## IMPORTANTE TENER LA APP REGISTRADA EN SETTINGS.PY, EN INSTALLED_APPS
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'usuarios',  # nuestra app de usuarios (python manage.py startapp usuarios)
    'plataformas', # mi app startapp plataformas (python manage.py startapp plataformas)
    'categorias', # mi app startapp categorias (python manage.py startapp categorias)
    'juegos', # mi app startapp juegos (python manage.py startapp juegos)
    'seeders', # mi app startapp seeders (python manage.py startapp seeders)
]
```
4. Ahora por ejemplo ya podemos ir creando un seed en en la carpeta managament/commands de la app seeders
- seed_juegos.py
```python
from django.core.management.base import BaseCommand
from juegos.models import Juego, Juegos_Plataformas, Fotos_Juegos
from categorias.models import Categoria
from plataformas.models import Plataforma

class Command(BaseCommand):
    help = 'Crea 12 juegos de ejemplo con plataformas y fotos'

    def handle(self, *args, **options):
        plataformas = {p.tipo: p for p in Plataforma.objects.all()}
        categorias = {c.nombre_categoria: c for c in Categoria.objects.all()}

        juegos_data = [
            {
                "titulo": "Dragon Ball Sparking Zero",
                "descripcion": "El nuevo juego de lucha de Dragon Ball para nueva generación.",
                "precio": 69.99,
                "stock": 20,
                "fecha_lanzamiento": "2024-10-01",
                "pegi": 12,
                "categoria": categorias.get("LUCHAS"),
                "plataformas": ["PLAYSTATION"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/231/231233.png"  # Dragon Ball Sparking Zero portada GAME
                ]
            },
            {
                "titulo": "Dragon Ball Xenoverse 2",
                "descripcion": "Continúa la historia de los patrulleros del tiempo.",
                "precio": 39.99,
                "stock": 15,
                "fecha_lanzamiento": "2016-10-28",
                "pegi": 12,
                "categoria": categorias.get("LUCHAS"),
                "plataformas": ["PLAYSTATION"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/143/143365.png"  # Dragon Ball Xenoverse 2 portada GAME
                ]
            },
            {
                "titulo": "Naruto Shippuden: Ultimate Ninja Storm 4",
                "descripcion": "El combate ninja definitivo.",
                "precio": 29.99,
                "stock": 10,
                "fecha_lanzamiento": "2016-02-05",
                "pegi": 12,
                "categoria": categorias.get("LUCHAS"),
                "plataformas": ["PLAYSTATION", "PC"],
                "fotos": [
                    "https://shared.fastly.steamstatic.com/store_item_assets/steam/apps/349040/header.jpg?t=1703080866"  # Naruto Storm 4 portada GAME
                ]
            },
            {
                "titulo": "EA Sports FC 26",
                "descripcion": "El simulador de fútbol más realista.",
                "precio": 69.99,
                "stock": 30,
                "fecha_lanzamiento": "2025-09-20",
                "pegi": 3,
                "categoria": categorias.get("DEPORTES"),
                "plataformas": ["PLAYSTATION", "PC", "XBOX"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/230/230123.png"  # EA Sports FC 24 portada GAME (no existe FC 26 aún)
                ]
            },
            {
                "titulo": "The Legend of Zelda: Breath of the Wild",
                "descripcion": "Una aventura épica en Hyrule.",
                "precio": 59.99,
                "stock": 12,
                "fecha_lanzamiento": "2017-03-03",
                "pegi": 12,
                "categoria": categorias.get("AVENTURAS"),
                "plataformas": ["NINTENDO"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/140/140634.png"  # Zelda Breath of the Wild portada GAME
                ]
            },
            {
                "titulo": "Minecraft",
                "descripcion": "Construye y explora mundos infinitos.",
                "precio": 26.95,
                "stock": 50,
                "fecha_lanzamiento": "2011-11-18",
                "pegi": 7,
                "categoria": categorias.get("SANDBOX"),
                "plataformas": ["PC", "PLAYSTATION", "XBOX", "NINTENDO"],
                "fotos": [
                    "https://gaming-cdn.com/images/products/442/616x353/minecraft-java-bedrock-edition-java-bedrock-edition-pc-juego-cover.jpg?v=1716387513"  # Minecraft Switch portada GAME
                ]
            },
            {
                "titulo": "Grand Theft Auto V",
                "descripcion": "Acción y crimen en Los Santos.",
                "precio": 29.99,
                "stock": 25,
                "fecha_lanzamiento": "2013-09-17",
                "pegi": 18,
                "categoria": categorias.get("ACCION"),
                "plataformas": ["PC", "PLAYSTATION", "XBOX"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/123/123936.png"  # GTA V portada GAME
                ]
            },
            {
                "titulo": "Forza Horizon 5",
                "descripcion": "Carreras en mundo abierto en México.",
                "precio": 59.99,
                "stock": 18,
                "fecha_lanzamiento": "2021-11-09",
                "pegi": 3,
                "categoria": categorias.get("CARRERAS"),
                "plataformas": ["PC", "XBOX"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/222/222530.png"  # Forza Horizon 5 portada GAME
                ]
            },
            {
                "titulo": "Animal Crossing: New Horizons",
                "descripcion": "Crea tu isla y haz nuevos amigos.",
                "precio": 49.99,
                "stock": 20,
                "fecha_lanzamiento": "2020-03-20",
                "pegi": 3,
                "categoria": categorias.get("SIMULACION"),
                "plataformas": ["NINTENDO"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/163/163296.png"  # Animal Crossing portada GAME
                ]
            },
            {
                "titulo": "The Witcher 3: Wild Hunt",
                "descripcion": "Una aventura de rol épica.",
                "precio": 39.99,
                "stock": 14,
                "fecha_lanzamiento": "2015-05-19",
                "pegi": 18,
                "categoria": categorias.get("RPG"),
                "plataformas": ["PC", "PLAYSTATION", "XBOX", "NINTENDO"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/140/140636.png"  # The Witcher 3 portada GAME
                ]
            },
            {
                "titulo": "Super Mario Odyssey",
                "descripcion": "La gran aventura de Mario en 3D.",
                "precio": 59.99,
                "stock": 16,
                "fecha_lanzamiento": "2017-10-27",
                "pegi": 7,
                "categoria": categorias.get("PLATAFORMAS"),
                "plataformas": ["NINTENDO"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/144/144861.png"  # Super Mario Odyssey portada GAME
                ]
            },
            {
                "titulo": "Call of Duty: Modern Warfare II",
                "descripcion": "Acción bélica moderna.",
                "precio": 69.99,
                "stock": 22,
                "fecha_lanzamiento": "2022-10-28",
                "pegi": 18,
                "categoria": categorias.get("SHOOTER"),
                "plataformas": ["PC", "PLAYSTATION", "XBOX"],
                "fotos": [
                    "https://media.game.es/COVERV2/3D_L/228/228377.png"  # Call of Duty MWII portada GAME
                ]
            },
        ]

        for juego_data in juegos_data:
            juego, created = Juego.objects.get_or_create(
                titulo=juego_data["titulo"],
                defaults={
                    "descripcion": juego_data["descripcion"],
                    "precio": juego_data["precio"],
                    "stock": juego_data["stock"],
                    "fecha_lanzamiento": juego_data["fecha_lanzamiento"],
                    "pegi": juego_data["pegi"],
                    "categoria": juego_data["categoria"],
                }
            )
            if created:
                self.stdout.write(self.style.SUCCESS(f"Juego '{juego.titulo}' creado."))
            else:
                self.stdout.write(self.style.WARNING(f"Juego '{juego.titulo}' ya existe."))

            # Relacionar con plataformas
            for plat in juego_data["plataformas"]:
                plataforma = plataformas.get(plat)
                if plataforma:
                    Juegos_Plataformas.objects.get_or_create(juego=juego, plataforma=plataforma)

            # Eliminar fotos antiguas y añadir las nuevas
            # cada vez que se corre el seeder se actualizan las fotos
            Fotos_Juegos.objects.filter(juego=juego).delete()
            for url in juego_data["fotos"]:
                Fotos_Juegos.objects.create(juego=juego, url=url)
                self.stdout.write(self.style.SUCCESS(f"Foto añadida a '{juego.titulo}': {url}"))
```
5. Ahora, donde está el manage.py, ya podemos ejecutar el seed.py, que es el seeder maestro
```python
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ ls
backend  categorias  db.sqlite3  juegos  manage.py  plataformas  seeders  usuarios
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py seed
Iniciando seeders...
Categoría 'Deportes' ya existe.
Categoría 'Luchas' ya existe.
Categoría 'Carreras' ya existe.
Categoría 'Acción' ya existe.
Categoría 'Aventuras' ya existe.
Categoría 'Rol (RPG)' ya existe.
Categoría 'Estrategia' ya existe.
Categoría 'Simulación' ya existe.
Categoría 'Puzzle' ya existe.
Categoría 'Terror' ya existe.
Categoría 'Shooter' ya existe.
Categoría 'Plataformas' ya existe.
Categoría 'Musical' ya existe.
Categoría 'Party' ya existe.
Categoría 'Sandbox' ya existe.
Categoría 'MMO' ya existe.
Categoría 'Educativo' ya existe.
Categoría 'Otros' ya existe.
Plataforma 'PC' ya existe.
Plataforma 'PlayStation' creada.
Plataforma 'Xbox' creada.
Plataforma 'Nintendo' ya existe.
El usuario 'havengame' ya existe.
El usuario 'usuario1' ya existe.
Juego 'Dragon Ball Sparking Zero' ya existe.
Foto añadida a 'Dragon Ball Sparking Zero': https://media.game.es/COVERV2/3D_L/231/231233.png
Juego 'Dragon Ball Xenoverse 2' creado.
Foto añadida a 'Dragon Ball Xenoverse 2': https://media.game.es/COVERV2/3D_L/143/143365.png
Juego 'Naruto Shippuden: Ultimate Ninja Storm 4' ya existe.
Foto añadida a 'Naruto Shippuden: Ultimate Ninja Storm 4': https://shared.fastly.steamstatic.com/store_item_assets/steam/apps/349040/header.jpg?t=1703080866
Juego 'EA Sports FC 26' ya existe.
Foto añadida a 'EA Sports FC 26': https://media.game.es/COVERV2/3D_L/230/230123.png
Juego 'The Legend of Zelda: Breath of the Wild' ya existe.
Foto añadida a 'The Legend of Zelda: Breath of the Wild': https://media.game.es/COVERV2/3D_L/140/140634.png
Juego 'Minecraft' creado.
Foto añadida a 'Minecraft': https://gaming-cdn.com/images/products/442/616x353/minecraft-java-bedrock-edition-java-bedrock-edition-pc-juego-cover.jpg?v=1716387513
Juego 'Grand Theft Auto V' ya existe.
Foto añadida a 'Grand Theft Auto V': https://media.game.es/COVERV2/3D_L/123/123936.png
Juego 'Forza Horizon 5' ya existe.
Foto añadida a 'Forza Horizon 5': https://media.game.es/COVERV2/3D_L/222/222530.png
Juego 'Animal Crossing: New Horizons' creado.
Foto añadida a 'Animal Crossing: New Horizons': https://media.game.es/COVERV2/3D_L/163/163296.png
Juego 'The Witcher 3: Wild Hunt' creado.
Foto añadida a 'The Witcher 3: Wild Hunt': https://media.game.es/COVERV2/3D_L/140/140636.png
Juego 'Super Mario Odyssey' creado.
Foto añadida a 'Super Mario Odyssey': https://media.game.es/COVERV2/3D_L/144/144861.png
Juego 'Call of Duty: Modern Warfare II' creado.
Foto añadida a 'Call of Duty: Modern Warfare II': https://media.game.es/COVERV2/3D_L/228/228377.png
Seeders ejecutados correctamente
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```
- Tambien podriamos ejecutar un seed individualmente como por ejemplo seed_juegos.py
```python
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py seed_juegos
Juego 'Dragon Ball Sparking Zero' ya existe.
Foto añadida a 'Dragon Ball Sparking Zero': https://media.game.es/COVERV2/3D_L/231/231233.png
Juego 'Dragon Ball Xenoverse 2' ya existe.
Foto añadida a 'Dragon Ball Xenoverse 2': https://media.game.es/COVERV2/3D_L/143/143365.png
Juego 'Naruto Shippuden: Ultimate Ninja Storm 4' ya existe.
Foto añadida a 'Naruto Shippuden: Ultimate Ninja Storm 4': https://shared.fastly.steamstatic.com/store_item_assets/steam/apps/349040/header.jpg?t=1703080866
Juego 'EA Sports FC 26' ya existe.
Foto añadida a 'EA Sports FC 26': https://media.game.es/COVERV2/3D_L/230/230123.png
Juego 'The Legend of Zelda: Breath of the Wild' ya existe.
Foto añadida a 'The Legend of Zelda: Breath of the Wild': https://media.game.es/COVERV2/3D_L/140/140634.png
Juego 'Minecraft' ya existe.
Foto añadida a 'Minecraft': https://gaming-cdn.com/images/products/442/616x353/minecraft-java-bedrock-edition-java-bedrock-edition-pc-juego-cover.jpg?v=1716387513
Juego 'Grand Theft Auto V' ya existe.
Foto añadida a 'Grand Theft Auto V': https://media.game.es/COVERV2/3D_L/123/123936.png
Juego 'Forza Horizon 5' ya existe.
Foto añadida a 'Forza Horizon 5': https://media.game.es/COVERV2/3D_L/222/222530.png
Juego 'Animal Crossing: New Horizons' ya existe.
Foto añadida a 'Animal Crossing: New Horizons': https://media.game.es/COVERV2/3D_L/163/163296.png
Juego 'The Witcher 3: Wild Hunt' ya existe.
Foto añadida a 'The Witcher 3: Wild Hunt': https://media.game.es/COVERV2/3D_L/140/140636.png
Juego 'Super Mario Odyssey' ya existe.
Foto añadida a 'Super Mario Odyssey': https://media.game.es/COVERV2/3D_L/144/144861.png
Juego 'Call of Duty: Modern Warfare II' ya existe.
Foto añadida a 'Call of Duty: Modern Warfare II': https://media.game.es/COVERV2/3D_L/228/228377.png
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```

6. Ahora creamos un comando fresh, para refrescar toda la base de datos
- python manage.py fresh (similar a php artisan migrate:fresh o php artisan migrate:Fresh --seed)
- Creamos el fichero seeders/management/commands/fresh.py
- Acorde a nuestro base de datos que es PostgreSQL
```python
from django.core.management.base import BaseCommand
from django.core.management import call_command
from django.db import connection

class Command(BaseCommand):
    help = "Reinicia la base de datos, migra y ejecuta los seeders (tipo migrate:fresh --seed)"

    def handle(self, *args, **kwargs):
        self.stdout.write(self.style.WARNING("Eliminando todas las tablas de PostgreSQL..."))

        with connection.cursor() as cursor:
            cursor.execute("""
                DO $$ DECLARE
                    r RECORD;
                BEGIN
                    FOR r IN (SELECT tablename FROM pg_tables WHERE schemaname = 'public') LOOP
                        EXECUTE 'DROP TABLE IF EXISTS ' || quote_ident(r.tablename) || ' CASCADE';
                    END LOOP;
                END $$;
            """)

        self.stdout.write(self.style.SUCCESS("Todas las tablas eliminadas."))

        # Migrar de nuevo
        self.stdout.write(self.style.WARNING("Ejecutando migraciones..."))
        call_command("migrate")

        # Ejecutar seeders
        self.stdout.write(self.style.WARNING("Ejecutando seeders..."))
        call_command("seed")

        self.stdout.write(self.style.SUCCESS("Base de datos reiniciada y seeders ejecutados correctamente."))
```

- Salida del terminal al hacer -> python manage.py fresh
```bash
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$ python manage.py fresh
Eliminando todas las tablas de PostgreSQL...
Todas las tablas eliminadas.
Ejecutando migraciones...
Operations to perform:
  Apply all migrations: admin, auth, categorias, contenttypes, juegos, plataformas, sessions, usuarios
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
  Applying categorias.0001_initial... OK
  Applying plataformas.0001_initial... OK
  Applying juegos.0001_initial... OK
  Applying juegos.0002_juegos_plataformas... OK
  Applying juegos.0003_fotos_juegos... OK
  Applying juegos.0004_alter_fotos_juegos_table... OK
  Applying sessions.0001_initial... OK
  Applying usuarios.0002_alter_modelousuariomodificado_options_and_more... OK
Ejecutando seeders...
Iniciando seeders...
Categoría 'Deportes' creada.
Categoría 'Luchas' creada.
Categoría 'Carreras' creada.
Categoría 'Acción' creada.
Categoría 'Aventuras' creada.
Categoría 'Rol (RPG)' creada.
Categoría 'Estrategia' creada.
Categoría 'Simulación' creada.
Categoría 'Puzzle' creada.
Categoría 'Terror' creada.
Categoría 'Shooter' creada.
Categoría 'Plataformas' creada.
Categoría 'Musical' creada.
Categoría 'Party' creada.
Categoría 'Sandbox' creada.
Categoría 'MMO' creada.
Categoría 'Educativo' creada.
Categoría 'Otros' creada.
Plataforma 'PC' creada.
Plataforma 'PlayStation' creada.
Plataforma 'Xbox' creada.
Plataforma 'Nintendo' creada.
Usuario 'havengame' creado exitosamente.
Usuario 'usuario1' creado exitosamente.
Juego 'Dragon Ball Sparking Zero' creado.
Foto añadida a 'Dragon Ball Sparking Zero': https://media.game.es/COVERV2/3D_L/231/231233.png
Juego 'Dragon Ball Xenoverse 2' creado.
Foto añadida a 'Dragon Ball Xenoverse 2': https://media.game.es/COVERV2/3D_L/143/143365.png
Juego 'Naruto Shippuden: Ultimate Ninja Storm 4' creado.
Foto añadida a 'Naruto Shippuden: Ultimate Ninja Storm 4': https://shared.fastly.steamstatic.com/store_item_assets/steam/apps/349040/header.jpg?t=1703080866
Juego 'EA Sports FC 26' creado.
Foto añadida a 'EA Sports FC 26': https://media.game.es/COVERV2/3D_L/230/230123.png
Juego 'The Legend of Zelda: Breath of the Wild' creado.
Foto añadida a 'The Legend of Zelda: Breath of the Wild': https://media.game.es/COVERV2/3D_L/140/140634.png
Juego 'Minecraft' creado.
Foto añadida a 'Minecraft': https://gaming-cdn.com/images/products/442/616x353/minecraft-java-bedrock-edition-java-bedrock-edition-pc-juego-cover.jpg?v=1716387513
Juego 'Grand Theft Auto V' creado.
Foto añadida a 'Grand Theft Auto V': https://media.game.es/COVERV2/3D_L/123/123936.png
Juego 'Forza Horizon 5' creado.
Foto añadida a 'Forza Horizon 5': https://media.game.es/COVERV2/3D_L/222/222530.png
Juego 'Animal Crossing: New Horizons' creado.
Foto añadida a 'Animal Crossing: New Horizons': https://media.game.es/COVERV2/3D_L/163/163296.png
Juego 'The Witcher 3: Wild Hunt' creado.
Foto añadida a 'The Witcher 3: Wild Hunt': https://media.game.es/COVERV2/3D_L/140/140636.png
Juego 'Super Mario Odyssey' creado.
Foto añadida a 'Super Mario Odyssey': https://media.game.es/COVERV2/3D_L/144/144861.png
Juego 'Call of Duty: Modern Warfare II' creado.
Foto añadida a 'Call of Duty: Modern Warfare II': https://media.game.es/COVERV2/3D_L/228/228377.png
Seeders ejecutados correctamente
Base de datos reiniciada y seeders ejecutados correctamente.
(env) joel-erreyes:~/docsjoel/proyectos personales/GameHaven/backend/backend_django$
```

