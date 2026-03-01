# Solucionar error de cors si nos da
1. Instalar dependencia de django-cors-headers
```bash
pip install django-cors-headers
```
- Lo añadimos al requeriments.txt 
```bash
pip freeze > requirements.txt
```
2. En settings.py, añadir en INSTALLED_APPS corsheaders
```python
    INSTALLED_APPS = [
    # ...otros apps...
    'corsheaders',
    # ...otros apps...
]
```
3. Agrear el middleware de cors al principio de la lista de MIDDLEWARE
```python
MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
    # ...el resto de middlewares...
]
```
4. Settings.py, permitir el origen de mi front end (localhost:4200)
```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:4200",
    "http://127.0.0.1:4200",
]
```
**Y ya, estaria, error de CORS solucionado!!!**

### importante si usamos cookies

- En settings-py, tener eso
- Para que el backend acepte y envie cookies en solicitudes cors desde el front end
- En resumen, para que el navegador incluya las cookies en las peticiones entre dominios
```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:4200",
    "http://127.0.0.1:4200",
]

CORS_ALLOW_CREDENTIALS = True # ESE PAREMETRO ES EL IMPORTANTE
```

