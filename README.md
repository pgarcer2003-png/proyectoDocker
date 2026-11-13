# proyectoDocker

## Preguntas de la entrega
1. ¿Por qué el frontend no necesita conectarse a PostgreSQL?
- El frontend pide los datos a la API. El backend recibe esas peticiones y se encarga de leer o guardar las incidencias en PostgreSQL.
2. ¿Por qué el backend pertenece a dos redes?
- Comparte la red frontend con Nginx para recibir las peticiones de la web. También comparte la red datos con PostgreSQL para poder consultar y guardar datos.
3. ¿Qué servicio necesita publicar un puerto en el anfitrión?
- El frontend. Publica el puerto 8080 del contenedor en el puerto indicado por PUERTO_WEB de la máquina anfitriona. Con la configuración de ejemplo se accede por localhost:8080.
4. ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?
- Publicar el puerto permite acceder a PostgreSQL desde el anfitrión según la dirección configurada. Si el backend falla al conectarse, hay que revisar que los dos servicios compartan la red datos, que el backend use db:5432 y que PostgreSQL esté listo y acepte sus credenciales.
5. ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?
- Deja por escrito la configuración de los servicios, sus redes, el volumen y las dependencias. Así podemos levantar todo el proyecto con docker compose up -d y repetir el despliegue con la misma configuración.
6. ¿Por qué hemos dividido la arquitectura en dos redes?
- Así cada servicio se comunica con los que necesita. La web y la API comparten una red, mientras que la API y PostgreSQL comparten otra. Eso evita que la web tenga acceso directo a PostgreSQL.
7. ¿Por qué el backend utiliza db como dirección de PostgreSQL?
- db es el nombre del servicio de PostgreSQL. Docker lo resuelve dentro de la red datos, aunque cambie la IP del contenedor. Desde el backend, localhost apunta al propio contenedor del backend.
8. ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?
- docker compose stop para los contenedores y conserva sus datos. docker compose down elimina los contenedores y las redes del proyecto, pero conserva el volumen postgres_data. Si ejecutamos docker compose down -v, también se elimina ese volumen y los datos guardados en él.
9. ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?
- La empresa perdería las incidencias y su historial guardado en la base de datos. Tendría que restaurar una copia de seguridad. Sin una copia, tocaría reconstruir a mano los registros que se puedan recuperar.
