
%% hola %%

___
-  [ ] Creamos un proyecto nuevo [[1-Inicio de un proyecto en Django]]

-  [ ] Entramos al proyecto con cd abrimos visual y creamos una carpeta con mkdir para alojar allí el HTML en este caso y en la mayoría lo llamaremos templates

-  [ ] Y creamos la app #app 

-  [ ] configuramos en el settings del proyecto y en "templates" en la variable dirs escribimos en los corchetes: #dirs==os.path.join(BASE_DIR, 'templates')== e importamos os ósea ==import os==

>[!nota]- que hace
esto une la ruta base_dir que es la base de nuestro proyecto y lo concatenamos con nuestra carpeta con los templates, en resumen esto permite tener un acceso directo a la carpeta especificada 

-  [ ] ahora vamos a la views en nuestra app para llamar el template con la función
```
def llamarTemplate(request):
	return render(request,)
```

 >[!nota]- orden de los templates
 >para tener mas ordenados los templates con las app creamos una carpeta en templates para saber a que app corresponde; luego de ==request,== ira la ruta de nuestro html como se ve en el código de abajo

-  [ ]  (en views) podemos mandar datos mediante una función como : 
```python
def llamarTemplate(request):
    data= {
        "nombre" : "el viejo del saco ",
        "arma" :    "saco de papa de 80kg "    }
    return render(request, 'primeraApp/untemplate.html',data)
```
    
-  [ ] para mostrar la información vamos al .html y la mostramos ejemplo:
```html
<body>
    <h1>RECOMPENSA</h1>
    <h2>Se busca a {{nombre}} mucho cuidado con su {{arma}}</h2>
    {% if edad %}
    <h2>su edad es: {{edad}}</h2>
    {%endif%}
</body>
```

 >[!nota]- if
 >sirve para mostrar la edad en caso de que exista

-  [ ] Para agregar archivos estáticos como una imagen creamos una carpeta nueva la llamamos static, ahora tenemos que dirigirnos a la ruta de la carpeta escribiendo en settings del proyecto ==STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]== en el html importamos =={%load static%}== y para colocar la imagen usamos:
```html
<img src="{%static "img/spiderman.webp"%}"alt="" heigth="200">
```
- hay opciones para comprimir la imagen y no comprometer tanto la calidad como **tinypng**
___
![[Pasted image 20230922204342.png]]

----
___ 
# Modelo Vista Template
___
---
-  [ ] creamos un archivo index.html en templates
-  [ ] en views creamos la función para la vista principal
```python
def index(request):
	return render(request,'productosApp/index.html')
```

-  [ ] Agregamos Bootstrap al html
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap demo</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
  </head>
  <body>
    <h1>Hello, world!</h1>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL" crossorigin="anonymous"></script>
  </body>
</html>
```

-  [ ] le agrega un alert e imagen importando static y necesitamos a en vez de button para agregar un href

```html
<body class="container">
	<div class="alert alert-info text-center display-1">
		<img src="{%static "img/logo.webp"%}"alt="" heigth="100px">
		Mi tienda django
	</div>
<div class="row"
	<div class="col-4 d-grid">
		<a class="btn btn-danger p-3">Consolas</a>
	</div>
	<div class="col-4 d-grid">
		<a class="btn btn-danger p-3">Electrodomesticos</a>
	</div>
	<div class="col-4 d-grid">
		<a class="btn btn-danger p-3">Ropa</a>
	</div>
</body>
```
-  [ ] agregamos la url siendo este un index sin olvidar importar views para que se vea el index, seria ==path( '  ',views.index)==
-  [ ] colocamos un href para redirigir a una pagina
```html
<body class="container">
	<div class="alert alert-info text-center display-1">
		<img src="{%static "img/logo.webp"%}"alt="" heigth="100px">
		Mi tienda django
	</div>
<div class="row"
	<div class="col-4 d-grid">
		<a class="btn btn-danger p-3" href="consolas">Consolas</a>
	</div>
	<div class="col-4 d-grid">
		<a class="btn btn-danger p-3" href="electro">Electrodomesticos</a>
	</div>
	<div class="col-4 d-grid">
		<a class="btn btn-danger p-3">Ropa</a>
	</div>
</body>
```
 >[!nota]-  href o name
 >por ahora utilizamos el href pero es una mala practica y deberíamos aprender a usar los name mas adelante

-  [ ] creamos un archivo para un template al que se direccionara nuestra pagina va a estar en la carpeta template productosApp y se llamara template.html
-  [ ] le creamos una funcion en views
```python
def consolas(request):
	return render(request,'productosApp/template.html')
```
-  [ ] le agregamos un diseño como el index ctrl+c
-  [ ] lo incluimos en url  y agregamos las demás vistas ==incluyendo su respectiva funcion en views==
```python
def consolas(request):
	data={
		'titulo':'Consolas'
	}
	return render(request,'productosApp/template.html,data')

urlpatterns=[
	path('admin').....,
	path('',views.index),
	path('consolas/',views,consolas),
	path('electrodomesticos/',views,electrodomesticos),
	path('ropa/',views,ropa),
]
```
-  [ ] en el template.html le agregamos un valor llamado titulo para solo cambiar la data en las funciones entonces las tres opciones electro ropa y consolas tienen un mismo html pero con diferente data 
```html
<div class="alert alert-info text-center display-1">
	<img src="{%static "img/logo.webp"%}"alt="" heigth="100px">
	Mi tienda django
</div>
<div class="alert alert-sucesstext-center display-6"
	{{titulo}}
</div>
```

[[3-Conexión a la base de datos]]