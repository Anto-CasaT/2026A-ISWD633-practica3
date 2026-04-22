## VOLUMEN NOMBRADO
Un volumen nombrado (named volume) es un tipo de volumen gestionado por Docker que se almacena en una ubicación específica del sistema de archivos del host y se identifica mediante un nombre único. Los volúmenes nombrados no requieren que especifiques una ruta del sistema de archivos del host, y en su lugar, Docker se encarga de la gestión y el almacenamiento del volumen.


### Crear volumen
```
docker volume create <nombre volumen>
```

### Crear el volumen nombrado: vol-postgres
```
docker volume create vol-postgres
```
<img width="736" height="125" alt="Captura de pantalla 2026-04-21 124512" src="https://github.com/user-attachments/assets/0f5c381f-eb7e-4955-98b5-68389182fa0f" />


## MOUNTPOINT
Un mountpoint se refiere al lugar en el sistema de archivos donde un dispositivo de almacenamiento se une (o monta) al sistema de archivos. Es el punto donde los archivos y directorios almacenados en ese dispositivo de almacenamiento son accesibles para el sistema operativo y las aplicaciones.

Por ejemplo, en Windows las unidades de almacenamiento (como `C:`, `D:`, etc.) actúan como puntos de montaje principales para discos duros, unidades flash, unidades ópticas y otros dispositivos de almacenamiento.

Cuando creas un volumen nombrado, Docker asigna un punto de montaje específico en el sistema de archivos del host para ese volumen.

### Estructura del Punto de Montaje:
- /var/lib/docker/volumes/: Es la ubicación base donde Docker almacena todos los volúmenes en el sistema de archivos del host.
- nombreVolumen/: Es el nombre del volumen nombrado que has creado. Docker crea un directorio con este nombre dentro de /var/lib/docker/volumes/ para almacenar los datos del volumen.
- _data: Es el subdirectorio dentro de vol-postgres/ donde se almacenan los datos reales del volumen. El nombre _data es una convención utilizada por Docker para indicar el directorio donde se encuentran los datos del volumen.

### ¿Cómo acceder a ese Mountpoint?
En el contexto de WSL (Windows Subsystem for Linux), wsl$ se refiere al nombre de un recurso compartido de red especial que representa la raíz del sistema de archivos de Windows desde WSL. Cuando accedes a \\wsl$ desde el Explorador de archivos de Windows, puedes ver y acceder a los archivos del sistema de archivos de la distribución de Linux en WSL.
\\wsl.localhost\docker-desktop-data\data\docker\volumes

### Crear un contenedor vinculado a un volumen nombrado
```
docker run -d --name <nombre contenedor> -v <nombre volumen>:<ruta contenedor> <nombre imagen>
```
ó
```
docker run -d --name <nombre contenedor> --mount type=volume,src=<nombre >,dst=<mount-path>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje. Para volúmenes con nombre, este es el nombre del volumen. Para volúmenes anónimos, este campo se omite.


### Crear la red net-drupal de tipo 
```
docker network create --driver bridge net-drupal
```
<img width="955" height="104" alt="Captura de pantalla 2026-04-21 124821" src="https://github.com/user-attachments/assets/abfe6264-8864-456e-b69a-a3286cec98c8" />


### Crear un servidor postgres vinculado a la red net-drupal, completar la ruta del contenedor
```
docker run -d --name server-postgres -e POSTGRES_DB=db_drupal -e POSTGRES_PASSWORD=12345 -e POSTGRES_USER=user_drupal --network net-drupal postgres
```
_No es necesario exponer el puerto, debido a que nos vamos a conectar desde la misma red de docker_

```
docker run -d --name server-postgres -e POSTGRES_DB=db_drupal -e POSTGRES_PASSWORD=12345 -e POSTGRES_USER=user_drupal --network net-drupal -v vol-postgres:/var/lib/postgresql/data postgres:16
```
<img width="1699" height="435" alt="Captura de pantalla 2026-04-21 145555" src="https://github.com/user-attachments/assets/0c05af8e-3712-4645-9c69-37a3d203b5b9" />


### Crear un cliente postgres vinculado a la red drupal a partir de la imagen dpage/pgadmin4, completar el correo
```
docker run -d --name client-postgres --publish published=9500,target=80 -e PGADMIN_DEFAULT_PASSWORD=54321 -e PGADMIN_DEFAULT_EMAIL=<correo> --network net-drupal dpage/pgadmin4
```

```
docker run -d --name client-postgres --publish published=9600,target=80 -e PGADMIN_DEFAULT_PASSWORD=54321 -e PGADMIN_DEFAULT_EMAIL=tu@correo.com --network net-drupal dpage/pgadmin4
```
<img width="1705" height="111" alt="Captura de pantalla 2026-04-21 145708" src="https://github.com/user-attachments/assets/036547e8-938d-4032-9455-cfd131f33ed9" />


### Usar el cliente postgres para conectarse al servidor postgres, para la conexión usar el nombre del servidor en lugar de la dirección IP.

<img width="1919" height="992" alt="Captura de pantalla 2026-04-21 152152" src="https://github.com/user-attachments/assets/7653d5e1-4e7d-4a29-a5c9-b0736aa653eb" />

### Crear los volúmenes necesarios para drupal, esto se puede encontrar en la documentación
```
docker volume create drupal-modules
docker volume create drupal-profiles
docker volume create drupal-themes
docker volume create drupal-sites
```
<img width="609" height="240" alt="image" src="https://github.com/user-attachments/assets/84451215-c349-47c1-9689-d02f69c8da26" />

### Crear el contenedor server-drupal vinculado a la red, usar la imagen drupal, y vincularlo a los volúmenes nombrados
```
docker run -d --name server-drupal --publish published=9700,target=80 -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> -v <nombre volumen>:<ruta contenedor> --network net-drupal drupal
```
<img width="1690" height="86" alt="image" src="https://github.com/user-attachments/assets/4e015ca1-4b2e-4c62-8a52-dbceb0cd0aaf" />

### Ingrese al server-drupal y siga el paso a paso para la instalación.
# COMPLETAR CON UNA CAPTURA DE PANTALLA DEL PASO 4

_La instalación puede tomar varios minutos, mientras espera realice un diagrama de los contenedores que ha creado en este apartado._
<img width="1918" height="1132" alt="image" src="https://github.com/user-attachments/assets/48e8dddc-e149-4513-aa26-e6ceb379e970" />
<img width="1456" height="814" alt="image" src="https://github.com/user-attachments/assets/130c0c5a-498e-49e2-84dd-a81b478c6d54" />


# COMPLETAR CON EL DIAGRAMA SOLICITADO
<img width="1356" height="503" alt="image" src="https://github.com/user-attachments/assets/60eee021-e3b1-4801-85be-ee5b1258dab4" />


### Eliminar un volumen específico
```
docker volume rm <nombre volumen>
```
**Considerar**
Datos Persistentes: Asegúrate de que el volumen no contiene datos críticos antes de eliminarlo, ya que esta operación no se puede deshacer.
Contenedores Activos: No puedes eliminar un volumen que está actualmente en uso por un contenedor activo. Debes detener y/o eliminar el contenedor primero.
