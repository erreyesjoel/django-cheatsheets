### Envio de correos reales con Django

1. Lo primero será, crear nuestra contraseña de aplicación -> https://support.google.com/accounts/answer/185833?hl=es | hacer click en -> "Crea y gestiona contraseñas de aplicacion" (le ponemos un nombre, le damos a crear y nos generará la "contraseña"
2. Ir a nuestro .env y poner estas dos variables, una para el correo que se está utilizando, y la contraseña generada

EMAIL_HOST_USER=correo@ejemplo.com
EMAIL_HOST_USER_PASSWORD=contraseñaTodoJunto
APP_NAME=nombre_remitente_que_queramos # lo que saldrá en el cuerpo del correo, la parte negrita al abrir el gmail, sin haber abierto el correo

3. Y en settings.py "usar" esas variables del .env, no se ponen como tal en settings.py por seguridad...
```.python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = env('EMAIL_HOST_USER')
EMAIL_HOST_PASSWORD = env('EMAIL_HOST_PASSWORD')
DEFAULT_FROM_EMAIL = EMAIL_HOST_USER
APP_NAME = env('APP_NAME')
```

4. Probar por shell, algo asi como en laravel con 'php artisan tinker', en pocas palabras, es para testear que la configuración es correcta y envia bien el correo al destinatario

- python manage.py shell
- una vez ahi, pegamos y damos enter para "enviar"
```bash
from django.core.mail import EmailMultiAlternatives
from django.conf import settings

subject = f"¡Bienvenido a {settings.APP_NAME}!"
from_email = f"{settings.APP_NAME} <{settings.EMAIL_HOST_USER}>"
to = ["erreyes.pilozo.joel.alejandro@alumnat.copernic.cat"]

# El primer texto plano es para clientes que no soportan HTML
text_content = f"Bienvenido a {settings.APP_NAME}.\nGracias por probar el envío de correos."
# El HTML permite que el nombre de la app salga en negrita en la vista previa
html_content = f"""
<p><strong>{settings.APP_NAME}</strong></p>
<p>Gracias por probar el envío de correos.</p>
"""

msg = EmailMultiAlternatives(subject, text_content, from_email, to)
msg.attach_alternative(html_content, "text/html")
msg.send()
```

## salida del terminal
```bash
joel-erreyes@joel-erreyes-Thin-15-B12UC:~/docsjoel/proyectos personales/EasyTrip/backend$ python manage.py shell
8 objects imported automatically (use -v 2 for details).

Python 3.12.3 (main, Feb  4 2025, 14:48:35) [GCC 13.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> # Abre el shell de Django:
>>> # filepath: instrucciones.txt
>>> # Ejecuta en terminal:
>>> # python manage.py shell
>>> 
>>> from django.core.mail import EmailMultiAlternatives
>>> from django.conf import settings
>>> 
>>> subject = f"¡Bienvenido a {settings.APP_NAME}!"
>>> from_email = f"{settings.APP_NAME} <{settings.EMAIL_HOST_USER}>"
>>> to = ["erreyes.pilozo.joel.alejandro@alumnat.copernic.cat"]
>>> 
>>> # El primer texto plano es para clientes que no soportan HTML
>>> text_content = f"Bienvenido a {settings.APP_NAME}.\nGracias por probar el envío de correos."
>>> # El HTML permite que el nombre de la app salga en negrita en la vista previa
>>> html_content = f"""
... <p><strong>{settings.APP_NAME}</strong></p>
... <p>Gracias por probar el envío de correos.</p>
... """
>>> 
>>> msg = EmailMultiAlternatives(subject, text_content, from_email, to)
>>> msg.attach_alternative(html_content, "text/html")
>>> msg.send()
1
```

### NOTA: EL 1 refleja exito

5. Verificar si ha enviado el correo o no (efectivamente nos lo ha enviado)

--

- Correo: 

EasyTrip
14:53 (fa 4 minuts)


EasyTrip

Gracias por probar el envío de correos.


