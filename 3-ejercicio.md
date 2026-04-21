## Esquema para el ejercicio
![Imagen](esquema-ejercicio3.PNG)

### Crear red net-wp
```
docker network ls
```
<img width="355" height="136" alt="image" src="https://github.com/user-attachments/assets/71770526-a5cd-4895-b752-e061740d7719" />

### Para que persista la información es necesario conocer en dónde mysql almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio carpeta del contenedor (a) es /var/lib/mysql
Ruta carpeta host: C:\Users\antoc\Documents\Sexto semestre\Construcción\ejercicio\db

### ¿Qué contiene la carpeta db del host?
Inicialmente la carpeta db está vacía, no contiene ningún archivo.

### Crear un contenedor con la imagen mysql:8  en la red net-wp, configurar las variables de entorno: MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER y MYSQL_PASSWORD
```
docker run -d --name srv-mysql-bm --network net-wp -v "C:\Users\antoc\Documents\Sexto semestre\Construcción\ejercicio\db":/var/lib/mysql -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=1234 mysql:8
```
<img width="1696" height="120" alt="image" src="https://github.com/user-attachments/assets/358317f1-eb4a-40c4-8c47-bfcbabb82292" />


### ¿Qué observa en la carpeta db que se encontraba inicialmente vacía?
La carpeta db ahora contiene los archivos de datos de MySQL: carpetas del 
sistema (mysql, performance_schema, sys), la base de datos wordpress, 
archivos de configuración (auto.cnf), archivos de datos (ibdata1) y archivos 
de log (binlog.000001, binlog.000002). Esto confirma que MySQL persiste sus 
datos en el host mediante el bind mount.
<img width="1919" height="732" alt="image" src="https://github.com/user-attachments/assets/5ce2d747-ffd4-4f7b-aa2d-72dd16b199b5" />


### Para que persista la información es necesario conocer en dónde wordpress almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio la carpeta del contenedor (b) es /var/www/html
Ruta carpeta host: C:\Users\antoc\Documents\Sexto semestre\Construcción\ejercicio\www

### Crear un contenedor con la imagen wordpress en la red net-wp, configurar las variables de entorno WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD y WORDPRESS_DB_NAME (los valores de estas variables corresponden a los del contenedor creado previamente)
```
docker run -d --name srv-wordpress-bm --network net-wp -p 9500:80 -v "C:\Users\antoc\Documents\Sexto semestre\Construcción\ejercicio\www":/var/www/html -e WORDPRESS_DB_HOST=srv-mysql-bm -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=1234 -e WORDPRESS_DB_NAME=wordpress wordpress
```
<img width="1692" height="121" alt="image" src="https://github.com/user-attachments/assets/92903d20-a602-4415-a9f1-086c3fcaffa6" />


### Personalizar la apariencia de wordpress y agregar una entrada
<img width="1919" height="1114" alt="image" src="https://github.com/user-attachments/assets/781c7580-0f12-4126-8d05-5c2263393398" />
<img width="1919" height="1083" alt="image" src="https://github.com/user-attachments/assets/e7e73c88-6e10-4e28-8fec-2ebcf82048ad" />


### Eliminar el contenedor y crearlo nuevamente, ¿qué ha sucedido?
Al eliminar el contenedor srv-wordpress-bm y recrearlo con el mismo comando, 
el sitio WordPress conserva todas las entradas ("Mi sitio WordPress" y 
"¡Hola mundo!"), el tema personalizado y toda la configuración. Esto sucede 
porque los archivos de WordPress están almacenados en la carpeta www del host 
mediante bind mount, y los datos de la base de datos persisten en la carpeta 
db también mediante bind mount. El ciclo de vida de los datos es completamente 
independiente del ciclo de vida del contenedor.
```
docker rm -f srv-wordpress-bm
```
<img width="684" height="57" alt="image" src="https://github.com/user-attachments/assets/63f181e0-9f39-427a-804c-175ecbb8f8e2" />

```
docker run -d --name srv-wordpress-bm --network net-wp -p 9500:80 -v "C:\Users\antoc\Documents\Sexto semestre\Construcción\ejercicio\www":/var/www/html -e WORDPRESS_DB_HOST=srv-mysql-bm -e WORDPRESS_DB_USER=wordpress -e WORDPRESS_DB_PASSWORD=1234 -e WORDPRESS_DB_NAME=wordpress wordpress
```
<img width="1713" height="136" alt="image" src="https://github.com/user-attachments/assets/7ea2a82a-fa61-46f3-a965-d017c758e369" />

<img width="1912" height="1100" alt="image" src="https://github.com/user-attachments/assets/2af8126a-2b13-44c2-a843-f883c698b1b3" />

