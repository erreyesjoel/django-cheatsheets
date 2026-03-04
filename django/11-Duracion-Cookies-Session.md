# Caducidad cookie de sesion
- En django templates tengo un login
- Al logearnos, se guarda nuestra sesion, en una cookie, la cookie por defecto en djagno se llama sessionid
- En settings.py, podemos definirle la caducidad, que por defecto si no ponemos nada, en principio es
- SESSION_COOKIE_AGE = 1209600  # 2 semanas en segundos
- En mi caso no tengo ni la variable, asi que en teoria pilla eso automatico, 2 semanas, debemos definirle una fecha de caducidad mas optima en terminos de seguridad
- Porque reduce el reisgo de que alguien acceda a una sesion activa si el usuario olvida cerrar sesion
- Limita el tiempo en que una sesion robada puede ser usada

## He optado por ponerle media hora de caducidad (30 minutos)
- en settings.py, al final del fichero por ejemplo
```python
SESSION_COOKIE_AGE = 1800  # 30 minutos en segundos
```
- Por si acaso, podemos reiniciar el servidor si lo estamos ejecutando
- En plan si hemos hecho -> python manage.py runserver


