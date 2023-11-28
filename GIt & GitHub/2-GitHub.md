
---

- ya teniendo todo previamente en el paso anterior tenemos que crear un repositorio en GitHub
- hacemos un  
```
git checkout main
```
- para colocarnos en la rama que queremos subir 
- agregamos nuestro git remoto con el http
```
git remote add origin https://github.com/JhinElForajido/LaConejaJuego.git
```

- podemos renombrar la rama
```
git branch -M main
```
- ahora empujamos la rama local llamada "main" al repositorio remoto
```
git push -u origin main
```
- si hay cambios en el repositorio remoto y lo hicimos nosotros y nos da un error podemos forzar el push
```
git push -u --force origin main
```
- ahora tenemos que identificarnos en la interfaz que aparece
- este seria nuestro codigo para enviar los commit hechos de manera local, teniendo en cuenta que tienes que realizar los commits locales para enviarlo actualizado  
- cuando agregamos el git remoto todos los push irán a ese repositorio no es necesario configurarlo nuevamente 

- descargar git remoto

```
git remote add Arielquezada123 https://github.com/MiguelCastillo142/EcoRevalorizeGitHub.git
```

```
git fetch Arielquezada123
```

```
git merge Arielquezada123/main
```
o en vez
```
git pull Arielquezada123 main
```

- para ver si hay cambios en el git remoto, y despues un **git** **status**
```
git fetch
```