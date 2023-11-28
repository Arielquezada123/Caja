[video que sigue](https://www.youtube.com/watch?v=QziV_gti04k&list=PLj7rmIknJU3MnO02g8b2xLbKDRthDNdu4&index=8)


___
-  [ ] #proj  Crear el proyecto en cmd con ==django-admin startproject holamundo==  en la carpeta deseada moviéndose entre directorios con cd y teniendo todo previamente instalado con ==pip install django== podemos ver esto con "django-admin" 

-  [ ]  Entramos a la carpeta creada con ==cd holamundo== y abrimos visual con ==code .==

-  [ ] #app Para crear la aplicación ==python ./manage.py startapp primerapp==

-  [ ] iniciamos el servidor ==python ./manage.py runserver== y entramos a la ruta local

-  [ ] Configuramos la hora del servidor en "settings" cambiamos "LANGUAGE_CODE='es-cl'" y "TIME_ZONE='America/Santiago'"

-  [ ] #app En ==INSTALLED_APPS== agregamos el nombre de nuestra aplicación (es keysensitive)

-  (OPCIONAL ) en views  para responder sin render importamos "from django.http import HttpResponse" y creamos nuestras funciones

-  [ ] Para agregar las urls nos vamos a la carpeta e importamos ejemplo: ==from primerapp import views== y agregamos la ruta como: ==path('saludar/',views.primeravista)==

[[2-Templates en Backend]]
____
