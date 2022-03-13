[My WepPage](http://yurihuallpa.com/)
# Comandos Docker de uso diario
- Descargar una imagen
```sh
docker pull {imagen}
```
> Note: `docker pull {imagen}:{version}` Descargar una versión específica.

- Muestra todas las imágenes descargadas en nuestro equipo 
```sh
docker images
```
> Note: `docker images | head` Muestra únicamente información de la cabecera (relevante).

- Muestra los contenedores en ejecución 
```sh
docker ps
```
> Note: `docker ps -a` Muestra el historial de contenedores que se han ejecutado. 

- Ejecutar un contenedor ya existente sin generar un nuevo ID
```sh
docker start {CONTAINER ID}
```
- Visualizar el log de un contenedor
```sh
docker log {CONTAINER ID}
```
> Note: `docker log -f {CONTAINER ID}` Visualizar el log de un contenedor pero manteniendo la interfaz interactiva.

- Ejecuta un comando dentro del contenedor en ejecución 
i:iteractive, t:terminal, sh:shell
```sh
docker exec -it {CONTAINER ID} sh
```
- Detener un contenedor en ejecución 
```sh
docker stop {CONTAINER ID}
```
- Ejecutar un contenedor en background (-d) d:detach
- Descargar una imagen si no existe y lo ejecuta
```sh
docker run -d {image}
```
> Note: `docker run {image}`Ejecuta una imagen, pero únicamente dentro del contenedor, entonces para hacer visible fuera del contenedor ejecutar el siguiente comando:` docker run -p {host port}:{docker port} {image}`.

- Generar una imagen
```sh
docker build .
```
> Note: `docker build -t {name-image} .` Genera una imagen con un nombre específico. 

- Ejecutar un contenedor con persistencia de estado, además los estados son bidireccionales(Cualquier modificación dentro del contenedor o en el directorio del host se vera reflejada en ambos) 
```sh
docker run -d -v {dir host}:/etc/todos -p {host port}:{docker port} {image name} .
```

- Subir una imagen a [Docker Hub](<https://hub.docker.com/>)
```sh
docker push {user name}/{image name}
```
## _Interacción entre contenedores_
1. Crear una red
```sh
docker network create {network name}
```
> Note: A partir de aquí ejecutar las imágenes en función de este nombre de red. Ejemplo: `docker run --network {network name} {....}`.

###### EJEMPLO
- Ejecutar la primera imagen (BASE DE DATOS)
> MYSQL: `docker run -d --network red-prueba --network-alias mysql -v current-dir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=databasePrueba mysql:5.7`.

- Ejecutar la segunda imagen (APLICACIÓN) 
> MYAPP: `docker run -dp 3030:3030 --network red-prueba -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=secret -e MYSQL_DB=databasePrueba aplicacion:v1`.

2. Utilizando docker-compose
- Crear el archivo docker-compose.yaml
```sh
vim docker-compose.yaml
```
- Modificar el archivo docker-compose.yaml y guardar
```sh
version: "1.0"
services:
    #docker run -dp 3030:3030 --network red-prueba -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=secret -e MYSQL_DB=databasePrueba aplicacion:v1
    app:
        image: aplicacion:v1
        ports:
            -3030:3030
        environment:
            MYSQL_HOST=mysql
            MYSQL_USER=root 
            MYSQL_PASSWORD=secret 
            MYSQL_DB=databasePrueba
    #docker run -d --network red-prueba --network-alias mysql -v current-dir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=databasePrueba mysql:5.7
    mysql:
        image: mysql:5.7
        volumes:
            - ./current-dir:/var/lib/mysql
        environment:
            MYSQL_ROOT_PASSWORD=secret 
            MYSQL_DATABASE=databasePrueba
```
- Ejecutar docker compose
```sh
docker-compose up -d
```
- Detener docker compose
```sh
docker-compose down
```

- [Docker] Ver más en la documentación de Docker
- [Youtube] Resumido del canal de youtube `Pelado Nerd`
## License

MIT

**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


 [Docker]: <https://docs.docker.com/reference/>
 [Youtube]: <https://www.youtube.com/watch?v=CV_Uf3Dq-EU&t=37s>
