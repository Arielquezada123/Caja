 con xampp
 
---
-  Abrimos **xampp** iniciamos **apache y MySQL**
- creamos nuestro proyecto #proj 
- Configuramos todo de settings y en base de datos o Databases para utilizar mysql instalamos en una terminal : 
```
pip install pymysql
```
- y en nuestro código arriba de Databases 
```python
import pymysql
pymysql.install_as_MySQLdb()
```
- ahora cambiamos el motor de base de datos predeterminado de Django que es SQLite por mysql y le damos el nombre de nuestra base de datos, también un usuario y password 
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_model',
        'USER' : 'root',
        'PASSWORD': '',
    }
}
```

>[!nota]- Dato
>Como se está usando una base de datos localhost no hay que configurar directorios ni direcciones a una base de datos en otro servidor
-  Nos vamos a la base de datos con admin y creamos nuestra base de datos con los mismos nombres ejemplo: **django_model**
- Ahora realizaremos las migraciones aunque no halla ningún cambio
```
python manage.py makemigrations
```

```
python manage.py migrate
```
- cuando queremos modificar un modelo y cargarlo borramos todos los pycache y hacemos un 
```
python manage.py makemigrations nombreapp
```
- ahora vamos a crear un modelo [[4-Crear modelo]]
