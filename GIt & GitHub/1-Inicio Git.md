---

---

---
- Instalamos git de su pagina [oficial](https://git-scm.com/)
- Nosotros vamos a utilizar un git bash
- Ahora vamos a decir a git quienes somos, esto es necesario o si no no podemos hacer nada
```bash
git config --global user.name "Ariel"

git config --global user.email "ariel.quezada06@inacapmail.cl"
```

>[!nota]- --global
>Esto significa que las configuraciones que estableces se aplicarán a todos los repositorios de Git en tu máquina, a menos que se especifiquen configuraciones específicas para un repositorio en particular. Esto es útil porque no necesitas ingresar tu nombre de usuario y correo electrónico cada vez que realizas un commit en un nuevo repositorio

- Para indicar a git que vamos a ejecutar un repositorio en un directorio  usamos `git init` en la carpeta 
 
![[Pasted image 20231115214647.png]]

- Esto nos dará un nombre a nuestra rama en este caso master, la cual podemos cambiar con 
```bash
git branch -m nombre
```
- Para ver el estado de la carpeta ejecutamos teniendo un archivo cualquiera
```bash
git status
```
- Para agregar al fichero que nos muestra en rojo diciendo que no está en el stage realizamos un:
```bash
git add nota.txt

git add .                            para agregar al stage todos los ficheros
```
- Lo que nos permitirá tener nuestro archivo listo para enviarlo, está preparando cambios para ser incluidos en el próximo commit
![[Pasted image 20231115220454.png]]
- Si ejecutamos ahora un:
```bash
git commit
```

![[Pasted image 20231116091339.png]]
- Nos mostrará una ventana nueva la cual nos pide un comentario al commit (podemos escribir al apretar la "i"), la cual podemos saltar al escribir un comentario en el codigo anterior como:

```bash
git commit -m "homeroxino"
```
- Así no veremos esta vista grafica
>[!nota]- -m
>De message ósea un comentario 

- Ahora veremos el git checkout y git reset 
![[Pasted image 20231116141958.png]]

![[Pasted image 20231116142057.png]]

- Si modificamos un archivo y no nos convence podemos hacer un checkout y volver al ultimo archivo de nuestra rama
```bash
git checkout  "GIt & GitHub/Inicio Git.md"
```

[[2-Conectar a GitHub]]