
- En nuestra app creamos un archivo forms.py
- Importamos forms ==from django import forms==
- Creamos una clase para registrar y otro para responder agregándole Bootstrap(lo de form-control)
```python
from django import forms
from modeloApp.models import Persona
class PersonaRegistrationForm(forms.Form):
    nombre = forms.CharField()
    correo = forms.CharField()
    telefono = forms.CharField()
    nombre.widget.attrs['class']='form-control'
    correo.widget.attrs['class']='form-control'
    telefono.widget.attrs['class']='form-control'
class PersonaRegistrationForm(forms.ModelForm):
    nombre = forms.CharField()
    correo = forms.CharField()
    telefono = forms.CharField()
    nombre.widget.attrs['class']='form-control'
    correo.widget.attrs['class']='form-control'
    telefono.widget.attrs['class']='form-control'
    class Meta:
        model=Persona
        fields='__all__'
```
- Creamos un html llamado formPersonas en el **template de la app**
- En **views** creamos una clase para registrar los datos del formulario ingresados
```python
def registrarPersona(request):
    form = PersonaRegistrationForm()
    if request.method=='POST':
        #los datos que vengan del post los va a rescatar
        form = PersonaRegistrationForm(request.POST)
        if form.is_valid():
            print("El formulario es valido")
            form.save()
            return  HttpResponseRedirect(reverse("listar_personas"))
    data = {'form' : form }
    return render (request, 'modeloApp/formPersona.html',data)
```
- El html del formulario se vería así 
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Formulario</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
  </head>
  <body class="container mt-5">
    <div class="alert alert-info display-3 text-center">Registro</div>
    <form method="POST">
        {%csrf_token%}
        {{form.as_p}}
        <input type="submit" value="Registrar persona" class="btn btn-info">
    </form>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL" crossorigin="anonymous"></script>
  </body>
</html>
```

>[!nota]- Dato 
>El método post para enviar el formulario a la base de datos y el csrf token es para temas de seguridad que nos da django para darle una clave encriptada a cada formulario y no se repita, el form-as_p es de Bootstrap para que se muestre como párrafo 

- Configuramos la url    ==path('addPersona/', registrarPersona),==
- El formulario se envía pero no se registra aun en la base de datos para hace esto vamos a forms.py 

```python
from django import forms
from modeloApp.models import Persona
class PersonaRegistrationForm(forms.Form):
    nombre = forms.CharField()
    correo = forms.CharField()
    telefono = forms.CharField()
class PersonaRegistrationForm(forms.ModelForm):
    class Meta:
        model=Persona
        fields='__all__'
```

>[!nota]- Dato 
>esto lo que hace es que los campos nombre correo y teléfono se van a considerar para crear un modelo que se asocie con el formulario

- Ahora en nuestra vista(views) le damos un form.save ósea que lo guarde
```python
from django.shortcuts import render
from modeloApp.models import Persona
from modeloApp.forms import PersonaRegistrationForm
from django.urls import reverse
from django.http import HttpResponseRedirect

# Create your views here.
def personasData(request):
    #sql --> select * from persona;
    personas = Persona.objects.all()
    data = {'personas': personas}
    return render(request, 'modeloApp/vistaPersonas.html', data)
def registrarPersona(request):
    form = PersonaRegistrationForm()
    if request.method=='POST':
        #los datos que vengan del post los va a rescatar
        form = PersonaRegistrationForm(request.POST)
        if form.is_valid():
            print("El formulario es valido")
            form.save()
            return  HttpResponseRedirect(reverse("listar_personas"))
    data = {'form' : form }
    return render (request, 'modeloApp/formPersona.html',data)
```

>[!nota] Dato 
>El reverse se usa para gestionar a que ruta ir y evitar que al refrescar la pantalla se vuelvan a enviar los datos para eso importamos las ultimas dos columnas de arriba también debemos darle el name en la url a la dirección ==path('personas/',personasData, name="listar_personas"),== tambien otra opción seria limpiar los datos luego de guardarlos

- Ahora le agregaremos un estado a nuestro formulario que podrá ser activo o desactivo para esto vamos a models.py y a nuestro modelo le agregamos la variable
```python
estado=models.CharField(max_length=50)
``` 

>Y hacemos un makemigrations y un migrate, esto nos muestra un mensaje que dice que ya tenemos información ingresada ósea debimos realizar esto antes de ingresar información, su solución es : ir a la carpeta que dice migrations ("__ pycahe__" opcional pero se recomienda borrar también) y la borramos para luego hacer un makemigrations nuevamente.
 Si lanzamos el código makemigrations nos dará un **error** sobre la falta de la carpeta static para eso la creamos fuera de la app y de la carpeta proyecto. 
 Ahora para que nos detecte los cambios debemos agregarle la aplicación a la que le hemos  echo los cambios **==python manage.py makemigrations modeloApp==** y ahora tenemos que borrar la base de datos y crearla nuevamente, ahora realizamos el migrate y ahora tenemos el estado en la base de datos, no es la mejor opción pero es funcional.

- ahora en el **==forms.py==** le agregaremos los valores y estados un select  además de restricciones en donde el teléfono no es requerido para crear una cuenta, el correo se verá como una contraseña

```python
from django import forms
from modeloApp.models import Persona

class PersonaRegistrationForm(forms.Form):
    ESTADOS=[
        ('activo','ACTIVO'),('inactivo','INACTIVO')
    ]
    nombre = forms.CharField()
    correo = forms.CharField(widget=forms.PasswordInput)
    telefono = forms.CharField(required=False)
    estado = forms.CharField(widget=forms.Select(choices=ESTADOS))

    nombre.widget.attrs['class']='form-control'
    correo.widget.attrs['class']='form-control'
    telefono.widget.attrs['class']='form-control'

class PersonaRegistrationForm(forms.ModelForm):
    ESTADOS=[
    ('activo','ACTIVO'),('inactivo','INACTIVO')
    ]
    nombre = forms.CharField()
    correo = forms.CharField(widget=forms.PasswordInput)
    telefono = forms.CharField(required=False)
    estado = forms.CharField(widget=forms.Select(choices=ESTADOS))


    nombre.widget.attrs['class']='form-control'
    correo.widget.attrs['class']='form-control'
    telefono.widget.attrs['class']='form-control'
    estado.widget.attrs['class']='form-control'
    class Meta:
        model=Persona
        fields='__all__'
```

- Y mostramos el estado en el html `<td>{{persona.estado}}</td>`
## Validación
- vamos a hacer unas  validaciones al formulario para ello vamos a **forms.py** y importamos `from django.core import validators`
- y le vamos a dar un mínimo y un máximo de letras al nombre 
```python
    nombre = forms.CharField(
        validators=[
            validators.MinLengthValidator(5),
            validators.MaxLengthValidator(10)
        ]
    )
```
- También podemos crear nuestra propia validación personalizada con una función   
```python
    def clean_email(self):
        input_email=self.cleaned_data['email']
        if input_email.find('@')==-1:
            raise forms.ValidationError("El correo de tener un @")
        return input_email
```

# Navegar entre direcciones
---
- crearemos un link para ingresar otra persona en esta misma vista que vemos las personas creamos una dirección hacia el formulario `<a class="btn btn-outline-primary" href="{% url 'insertar_persona'%}">Agregar</a>` y en urls le damos el nombre que está en el código también lo podemos hacer en el formulario : `<a class="btn btn-outline-danger" href="{% url 'listar_personas'%}">Ver personas</a>`
---
# Vista de la tabla como Datatable
---
 - La información oficial está [aquí](https://datatables.net/)
 - Le damos un id a nuestra tabla en la vista
![[Pasted image 20231117233119.png]]
 - copiamos lo de cdn de la pagina de datatable que es como un Bootstrap pegamos el link en el head y el script abajo 
 ```html
<link rel="stylesheet" href="https://cdn.datatables.net/1.13.6/css/jquery.dataTables.css" />

<script src="https://cdn.datatables.net/1.13.6/js/jquery.dataTables.js"></script>
 ```
 - ahora buscamos en el navegador **==cdn jquery==** elegimos uno y copiamos su enlace de slim minified
 - ahora creamos un script en el html un dentro un src y el link
```html
<script src="https://code.jquery.com/jquery-3.7.1.slim.min.js"></script>
```
- creamos otro script para que llame a datatable(está en la pagina de datatable)
```python
`$(document).ready(` `function` `() {`

    `$(``'#myTable'``).DataTable();`

`} );`
```
y le colocamos el nombre de nuestra tabla
- el orden importa y debería verse así y el link arriba que no tiene orden 
 ![[Pasted image 20231029183731.png]]
- Quedando así
```html
<!doctype html>
<html lang="en">
<head>
  <title>Listado</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <link rel="stylesheet" href="https://cdn.datatables.net/1.13.6/css/jquery.dataTables.css" />
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet"
    integrity="sha384-iYQeCzEYFbKjA/T2uDLTpkwGzCiq6soy8tYaI1GyVh/UjpbCx/TYkiZhlZB6+fzT" crossorigin="anonymous">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css">
</head>
    <body class="container mt-6">
        <div class="alert alert-success display-3">listado</div>
        {%if personas %}
            <div class="table-responsive">
                <table class="table table-primary" id="miTabla">
                    <thead>
                        <tr>
                            <th scope="col">ID</th>
                            <th scope="col">Nombre</th>
                            <th scope="col">Correo</th>
                            <th scope="col">Telefono</th>
                            <th scope="col">Estado</th>
                            <th scope="col"></th>
                        </tr>
                    </thead>
                    <tbody>
                        {% for persona in personas %}
                            <tr class="ID">
                                <td scope="row">{{persona.id }}</td>    
                                <td>{{persona.nombre}}</td>
                                <td>{{persona.correo}}</td>
                                <td>{{persona.telefono}}</td>
                                <td>{{persona.estado}}</td>
                                <td>
                                    <a href="/editarPersona/{{persona.id}}" class="btn btn-warning"><i class="bi bi-universal-access"></i></a>
                                    <a href="/eliminarPersona/{{persona.id}}" class="btn btn-primary"><i class="bi bi-textarea-t"></i></a>
                                </td>
                            </tr>
                        {% endfor %}
                    </tbody>
                </table>
            </div>
        {% else %}
            <div class="alert alert-info ">no hay personas</div>
        {% endif %}
        <a class="btn btn-outline-primary" href="{% url 'insertar_persona'%}">Agregar</a>
  <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.11.6/dist/umd/popper.min.js"
    integrity="sha384-oBqDVmMz9ATKxIep9tiCxS/Z9fNfEXiDAYTujMAeBAsjFuCZSmKbSSUnQlmh/jp3" crossorigin="anonymous">
  </script>
  <script src="https://code.jquery.com/jquery-3.7.1.slim.min.js"></script>
  <script src="https://cdn.datatables.net/1.13.6/js/jquery.dataTables.js"></script>
  <script>
    $(document).ready(function () {
        $('#miTabla').DataTable();
    } );
  </script>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.min.js"
    integrity="sha384-7VPbUDkoPSGFnVtYi0QogXtr74QeVeeIs99Qfg5YCF+TidwNdjvaKZX19NZ/e6oz" crossorigin="anonymous">
  </script>
</body>
</html>
```
---
# Borrar y editar 
---
- Ingresamos una nueva columna en la tabla y sus botones 
```html
	<td>
		 <a href="" class="btn btn-warning">.</a>
		<a href="" class="btn btn-primary">x</a>
	</td>
```
- vamos a los iconos de Bootstrap pegamos el link del cdn en nuestro html  y elegimos un icono copiamos el código y pegamos donde está el punto y la x
- ahora en views hacemos una funcion que edite el empleado
```python
def editarPersona(request,id ):
    persona=Persona.objects.get(id = id )
    form=PersonaRegistrationForm(instance=persona)
    if (request.method=='POST'):
        form=PersonaRegistrationForm(request.POST,instance=persona)
        if form.is_valid():
            form.save()
        return  HttpResponseRedirect(reverse("listar_personas"))
    data = {'form' : form }
    return render (request, 'modeloApp/formPersona.html',data)
```
- y agregamos la url (hay que ver porque no actualiza)
```python
path('editarPersona/<int:id>', editarPersona, name="editarPersona"),
```
- para borrar hacemos la función 
```python
def eliminarPersona(request,id):
    persona=Persona.objects.get(id = id )
    persona.delete()
    return  HttpResponseRedirect(reverse("listar_personas"))
```
- y vamos a la url 
```python
path('eliminarPersona/<int:id>', eliminarPersona, name="eliminarPersona"),
```
- y direccionamos el botón del html
```html
<a href="/eliminarPersona/{{persona.id}}" class="btn btn-primary"><i class="bi bi-textarea-t"></i></a>
```

[[6-Login]]