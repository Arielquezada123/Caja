---

---
___
- Descargamos [gnuwget](https://eternallybored.org/misc/wget/)
- Movemos el .exe al system32
- Escribimos el siguiente comando en cmd estando en la carpeta a guardar la pagina 
```
wget --mirror -convert-links -adjust-extension --page-requisites --no-parent --domains w3schools.com --wait=2 https://www.w3schools.com

```