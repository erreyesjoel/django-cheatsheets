# Como cogemos los mensajes error de nuestro backend, en django templates
- En views.py, podemos definir una variable de error, en la def que toque, como error = None
```python
# def para renderizar el template de login
def login_template(request):
    # si el usuario esta autenticado, redirigir al dashboard
    # para que si esta autenticado, que no pueda volver al login sin cerrar sesion
    if request.user.is_authenticated:
        return redirect('admin_dashboard_template')
    error = None
    if request.method == 'POST':
        # cogemos los campos email y password
        email = request.POST.get('email')
        password = request.POST.get('password')
        # buscar el usuario por email
        try:
            user_obj = ModeloUsuarioModificado.objects.get(email=email)
            # autenticar usando el username del usuario encontrado
            user = authenticate(request, username=user_obj.username, password=password)
            if user is not None: # si el usuario es valido, osea sus credenciales son correctas, logeamos
                login(request, user)  # loguear al usuario
                return redirect('admin_dashboard_template')  # redirigir al dashboard de admin
            else:
                error = "Credenciales inválidas. Por favor, inténtalo de nuevo."
        except ModeloUsuarioModificado.DoesNotExist:
            error = "Credenciales inválidas. Por favor, inténtalo de nuevo."
    # si no es post, renderizamos el template de login
    return render(request, 'autenticacion/login.html', {'error': error})
```

- Y en el html, pillarla asi
```html
     {% if error %}
      {{ error }}
      {% endif %}
```

**Esa es una forma**

## La forma mas limpia es usando import messages de django.contrib

- en views.py, usamos el import de messages, con messages.error, podemos mostrar los mensajes de error en el html, en nuestro template
```python
from django.contrib import messages  # para mostrar mensajes flash en Django

# def para renderizar el template de login
def login_template(request):
    # si el usuario esta autenticado, redirigir al dashboard
    # para que si esta autenticado, que no pueda volver al login sin cerrar sesion
    if request.user.is_authenticated:
        return redirect('admin_dashboard_template')
    if request.method == 'POST':
        # cogemos los campos email y password
        email = request.POST.get('email')
        password = request.POST.get('password')
        # buscar el usuario por email
        try:
            user_obj = ModeloUsuarioModificado.objects.get(email=email)
            # autenticar usando el username del usuario encontrado
            user = authenticate(request, username=user_obj.username, password=password)
            if user is not None: # si el usuario es valido, osea sus credenciales son correctas, logeamos
                login(request, user)  # loguear al usuario
                return redirect('admin_dashboard_template')  # redirigir al dashboard de admin
            else:
                # si las credenciales no son válidas, mostramos mensaje de error usando messages
                messages.error(request, "Credenciales inválidas. Inténtalo de nuevo.")
                return redirect('login_template')  # redirigir para evitar el reenvío del formulario
        except ModeloUsuarioModificado.DoesNotExist:
            # si el usuario no existe, mostramos mensaje de error usando messages
            messages.error(request, "Credenciales inválidas. Inténtalo de nuevo.")
            return redirect('login_template')  # redirigir para evitar el reenvío del formulario
    # si no es post, renderizamos el template de login
    return render(request, 'autenticacion/login.html')
```


- y en el html
```html
    <!-- Mostrar mensajes de error de Django messages -->
    {% if messages %}
      {% for message in messages %}
        <div class="login-error">
          {{ message }}
        </div>
      {% endfor %}
```

