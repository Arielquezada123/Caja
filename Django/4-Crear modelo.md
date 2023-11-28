

---
- nos dirigimos a **models** de nuestra app 
>[!nota] Dato de los modelos
>al trabajar con modelos tenemos que importar en cada clase models.Model, Django también se encarga de darles un id autoincremental sin necesidad de solicitarlo 

- y creamos una clase con datos
```python
class Persona(models.Model):
    nombre = models.CharField(max_length=50)
    correo= models.CharField(max_length=30)
    telefono=models.CharField(max_length=13)
```
- para crear este modelo en la base de datos detenemos el servidor y en el terminal solicitamos una migración 
```
python manage.py makemigrations
```
y **aplicamos la migración**
```
python manage.py migrate
```
- vamos a crear una tabla en templates llamado formPersonas.html que mostrará la información de los usuarios  viéndose el formulario así :
```html
<!doctype html>
<html lang="en">
<head>
  <title>Title</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-iYQeCzEYFbKjA/T2uDLTpkwGzCiq6soy8tYaI1GyVh/UjpbCx/TYkiZhlZB6+fzT" crossorigin="anonymous">
</head>
    <body class="container mt-6">
        <div class="alert alert-success display-3">
            listado
        </div>
        {%if personas%}
            <div class="table-responsive">
                <table class="table table-primary" >
                    <thead>
                        <tr>
                            <th scope="col">ID</th>
                            <th scope="col">Nombre</th>
                            <th scope="col">correo</th>
                            <th scope="col">telefono</th>
                        </tr>
                    </thead>
                    <tbody>
                        {% for persona in persona %}
                            <tr class="ID">
                                <td scope="row">{{persona.id }}</td>    
                                <td>{{persona.nombre}}</td>
                                <td>{{persona.correo}}</td>
                                <td>{{persona.telefono}}</td>
                            </tr>
                        {% endfor %}
                    </tbody>
                </table>
            </div>
        {%else%}
            <div class="alert alert-info ">no hay personas</div>
        {%end if%}
  <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.11.6/dist/umd/popper.min.js"
    integrity="sha384-oBqDVmMz9ATKxIep9tiCxS/Z9fNfEXiDAYTujMAeBAsjFuCZSmKbSSUnQlmh/jp3" crossorigin="anonymous">
  </script>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.min.js"
    integrity="sha384-7VPbUDkoPSGFnVtYi0QogXtr74QeVeeIs99Qfg5YCF+TidwNdjvaKZX19NZ/e6oz" crossorigin="anonymous">
  </script>
</body>
</html>
```

>[!nota]- Dato 
>th es para el head de la tabla td de data , el código mismo dice lo que hace 
- ahora vamos a crear una vista en views en donde llamaremos a la base de datos
```python
from django.shortcuts import render
from modeloApp.models import Persona

def personasData(request):
    #sql --> select * from persona;
    personas = Persona.objects.all()
    data = {'personas':personas}
    return render(request, 'modeloApp/formPersonas.html', data)
```
- y lo agregamos a nuestras urls
```python
from django.contrib import admin
from django.urls import path
from modeloApp.views import personasData

urlpatterns = [
    path('admin/', admin.site.urls),
    path('personas/',personasData),
]
```

[[5-Formulario y Tablas]]