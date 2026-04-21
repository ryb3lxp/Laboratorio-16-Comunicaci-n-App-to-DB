# Laboratorio-16-Comunicaci-n-App-to-DB
Laboratorio-docker-networking
Laboratorio 16: Networking Lógico y Persistencia en Docker
1. Comandos del Laboratorio
Para completar el ejercicio de comunicación y persistencia, se ejecutaron los siguientes comandos:

Creación de la Red Definida por Software (SDN):
docker network create red-negocio 

Despliegue de Base de Datos con Persistencia:
docker run -d --name mi-db --network red-negocio -v mi-data-db:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secreto mariadb:latest 

Prueba de Resolución DNS (Service Discovery):
docker run -it --rm --network red-negocio alpine ping mi-db 

2. Topología del Lab 16
Diagrama Lógico: [ Contenedor App ] <---- (Red: red-negocio) ----> [ Contenedor mi-db ] 

El tráfico se aísla en una red privada para asegurar que la base de datos no tenga exposición pública directa.
Los servicios se comunican mediante el DNS interno de Docker, permitiendo que el nombre sea el ancla permanente mientras las IPs son efímeras.

3. Reflexiones e Investigación
A. Sobre la Red red-negocio
Pregunta: Si borro la red red-negocio, ¿qué sucede con los contenedores que estaban conectados a ella? 
Respuesta: Los contenedores seguirán existiendo, pero quedarán aislados. Perderán la capacidad de comunicarse entre sí mediante sus nombres de contenedor, ya que el servidor DNS interno de Docker solo resuelve nombres dentro de redes personalizadas.

B. Investigación sobre Persistencia
¿Qué es la Persistencia? 
Es la capacidad de que los datos sobrevivan más allá del ciclo de vida de un contenedor.
¿Por qué se borran los datos sin volúmenes? 
Los contenedores usan capas de escritura efímeras. Cuando el contenedor se destruye, esa capa desaparece. Los volúmenes actúan como un "ancla" en el disco físico del host.

C. Bind Mount vs. Managed Volume
Managed Volume: Docker gestiona la ubicación y los permisos en el host. Es el estándar para producción.
Bind Mount: Vincula una ruta específica del host elegida por el usuario al contenedor.

4. Reflexión sobre el uso en Producción
En producción, el uso de contenedores y volúmenes permite separar la aplicación de los datos, logrando un 99.5% de rendimiento del hardware frente a la virtualización tradicional. Esto permite densidades de despliegue 10 veces superiores y puede reducir costos de infraestructura en un 40%.


