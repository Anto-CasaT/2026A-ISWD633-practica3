# COMPLETAR  
Comparando sus conocimientos antes de hacer la práctica con sus conocimientos después de hacer la tarea, explicar los principales aprendizajes logrados para beneficio de su formación profesional.  
Si solucionó un problema presentado o utilizó otros comandos que no se mencionan al realizar la práctica también se debe documentar.

# Mi Aprendizaje

- Aprendí que un **bind mount** vincula una carpeta del host directamente 
al contenedor, por lo que cualquier cambio en la carpeta del host se 
refleja inmediatamente en el contenedor sin necesidad de reiniciarlo.

- Aprendí que un **volumen nombrado** es gestionado por Docker y persiste 
aunque el contenedor sea eliminado, a diferencia del bind mount que depende 
de una ruta específica del host.

- Aprendí que `docker volume create <nombre>` crea un volumen nombrado y 
`docker volume rm <nombre>` lo elimina.

- Aprendí que `docker volume prune` elimina todos los volúmenes anónimos 
que no están en uso por ningún contenedor.

- Aprendí que algunos contenedores como Drupal requieren inicializar el 
volumen sites con: `docker run --rm -v drupal-sites:/temporary/sites drupal 
cp -aRT /var/www/html/sites /temporary/sites` antes de crear el contenedor principal.

- Al crear el servidor postgres con la imagen latest (versión 18+) obtuve 
un error porque el mountpoint cambió. Lo solucié usando `postgres:16` que 
es compatible con `/var/lib/postgresql/data`.

- Aprendí que `docker rm -fv <contenedor>` elimina el contenedor junto con 
su volumen anónimo asociado.
