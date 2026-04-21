# BIND MOUNT
En un bind mount mapeamos (montar) un directorio o archivo específico del sistema de archivos del host con una parte del sistema de ficheros del contenedor.

```
docker run -d --name <nombre contenedor> -v <ruta carpeta host>:<ruta carpeta contenedor> <imagen> 
```
ó
```
docker run -d --name <nombre contenedor> --mount type=bind,source=<ruta carpeta host>,target=<ruta carpeta contenedor> <imagen>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje.
  
### En tu computador crear una carpeta llamada nginx y dentro de esta carpeta crea otra llamada html. Como se aprecia en la figura.
![Volúmenes](directorio.PNG)

### Crear un contenedor con la imagen nginx:alpine, mapear todos por puertos, para la ruta carpeta host colocar el directorio en donde se encuentra la carpeta html en tu computador y para la ruta carpeta contenedor: /usr/share/nginx/html (esta ruta se obtiene al revisar la documentación de la imagen)
![Volúmenes](volumen-host.PNG)
```
docker run -d --name srv-nginx-bind -P -v "C:\Users\antoc\Documents\Sexto semestre\Construcción\nginx\html":/usr/share/nginx/html nginx:alpine
```
<img width="1697" height="118" alt="image" src="https://github.com/user-attachments/assets/5695cec3-532e-4043-a2dd-0afb712b02c6" />

# COMPLETAR CON EL COMANDO

### ¿Qué sucede al ingresar al servidor de nginx?
Se muestra un error 403 Forbidden porque la carpeta html del host está vacía.
<img width="1919" height="457" alt="image" src="https://github.com/user-attachments/assets/9af7d614-fbe4-4b1b-9001-8d4cae4d9de4" />


### ¿Qué pasa con el archivo index.html del contenedor?
El archivo index.html original del contenedor es reemplazado por el contenido 
de la carpeta html del host. Al estar vacía, nginx no encuentra ningún archivo 
que mostrar y devuelve un error 403.


### Ir a https://html5up.net/ y descargar un template gratuito, descomprirlo dentro de tu computador en la carpeta html
### ¿Qué sucede al ingresar al servidor de nginx?
Se muestra el template Stellar de HTML5 UP porque nginx sirve los archivos 
que están en la carpeta html del host.
<img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/73abe2f2-9d9e-4f5d-8f04-86cc726cdd04" />


### Eliminar el contenedor
```
docker rm -f srv-nginx-bind
```
<img width="631" height="149" alt="image" src="https://github.com/user-attachments/assets/476ba0f2-fef5-4248-b000-fdebea7db4d0" />


### ¿Qué sucede al crear nuevamente un contenedor montado al directorio definidos anteriormente?
El template Stellar sigue visible porque los archivos están en la carpeta 
del host y no en el contenedor. Al recrear el contenedor con el mismo 
bind mount, nginx vuelve a servir los mismos archivos.
```
docker run -d --name srv-nginx-bind -P -v "C:\Users\antoc\Documents\Sexto semestre\Construcción\nginx\html":/usr/share/nginx/html nginx:alpine
```
<img width="1907" height="1119" alt="image" src="https://github.com/user-attachments/assets/d530d77f-7bc0-4e20-ad86-32cb3e814199" />


