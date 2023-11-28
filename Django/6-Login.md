 ---
 
 >[!nota"]- DIRS 
 >#dirs en Templates de settings podemos escribir el dirs como `[BASE_DIR/"templates"]` en vez de importar os y hacer el codigo mas largo 
 
---
- Configuramos nuestro settings y creamos nuestras carpetas como templates y sin olvidar los archivos estáticos 
```python
STATICFILES_DIRS = [BASE_DIR/"static"]
```
- Para poder hacer un Login tenemos que tener importada la app `'django.contrib.auth',` en installed apps 
- Para poder trabajar con configuraciones url tenemos que usar la ultima opción en la que tenemos que importar include  
![[Pasted image 20231117225558.png]]

- Vamos a agregar todas las urls para las autentificaciones con :
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('cuentas/', include("django.contrib.auth.urls")),
]
```
- Hacemos un migrations 
- y si ejecutamos el proyecto nos muestra nuestra pagina con nuestra url llamada cuentas si entramos a `http://127.0.0.1:8000/cuentas` y luego escribimos cualquier cosa como `http://127.0.0.1:8000/cuentas/sk%C3%B1adkl` nos mostrará las direcciones a las que podemos llegar en este caso Login.
![[Pasted image 20231118160154.png]]
- si entramos a Login nos dice que nos falta un template para mostrar que tiene que estar en la carpeta **registration** / **login**.**html** entonces creamos la carpeta en templates y su html.
![[Pasted image 20231118181143.png]]
- vamos a crear el usuario para entrar a la sesión pero antes necesitamos un **super usuario** 
```
python manage.py createsuperuser
```
- y creamos un button y la seguridad 
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
</head>
<body>
    <form method="post">
    {% csrf_token %}
    {{form.as_p}}    
    <button type="submit">loguear</button>
    </form>
</body>
</html>
```
- ahora podemos entrar al perfil que creamos en el super usuario 
![[Pasted image 20231119125842.png]]

---
# Templates y mas Templates
---
- ahora vamos a realizar templates con un template esto lo realizamos teniendo un archivo base y solo vayamos llamando al archivo base
- para esto creamos dos htmls en nuestros templates en los cual tendremos uno de base y otro de vista home
![[Pasted image 20231120200233.png]]
- nuestro html base lo ocuparemos con un extend para el home 
- ahora en settings tenemos que informar hacia donde iremos luego de rellenar el Login
![[Pasted image 20231120215812.png]]
- En urls mostraremos el template home como index sin la necesidad de un view (por eso el codigo de template view)
![[Pasted image 20231120200344.png]]
- teniendo solo un extend en el archivo home nos mostrara lo del archivo "base.html"
![[Pasted image 20231120200556.png]]
- Le agregamos funcionalidad para que si el usuario esta logueado muestre un mensaje si no lo envía a iniciar sesión 
![[Pasted image 20231120215716.png]]
- para que el usuario entre a la dirección solo autenticado hacemos un Login required en el views de nuestra app
```
from django.shortcuts import render
from django.urls import reverse
from django.http import HttpResponseRedirect
from django.contrib.auth.decorators import login_required
@login_required(login_url='login')  
def postlogin(request):
    return render(request, 'postlogin.html')
```
- y le coloqué un nombre a cuentas en las urls
![[Pasted image 20231125182831.png]]

La etiqueta `{% include %}` se utiliza para incluir el contenido de otra plantilla en la posición donde se coloca en la plantilla actual. Puedes usar `{% include %}` dentro de bloques condicionales y en otros lugares para incluir dinámicamente contenido.

En cuanto a la cuestión del `{% extends %}`, normalmente se usa para definir una plantilla base única que se extiende en varias páginas. El error que mencionabas podría haber ocurrido si intentabas cambiar la plantilla base dinámicamente después de haber comenzado a procesar el archivo de la plantilla.