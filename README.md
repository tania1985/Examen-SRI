# Examen-SRI

## 1. Explica métodos para 'abrir' una consola/shell a un contenedor que se está ejecutando
 1. docker exec:
    - Utiliza el comando docker exec para ejecutar un comando dentro de un contenedor en ejecución.
    > Ejemplo:
        `docker exec -it nombre_del_contenedor /bin/bash`
 2. docker attach:
    - Utiliza el comando docker attach para conectarte a la terminal estándar del contenedor.
    > Ejemplo:
        `docker attach nombre_del_contenedor`
    - Ten en cuenta que docker attach no inicia un nuevo shell, sino que se conecta al proceso principal del contenedor. Puede no ser adecuado para todos los casos.
 3. Entrypoint o CMD con un shell interactivo:

    - Si estás creando tu propia imagen de Docker, asegúrate de que el ENTRYPOINT o CMD del Dockerfile incluya un shell interactivo, por ejemplo:
    > ENTRYPOINT ["/bin/bash"]
    - Luego, al ejecutar el contenedor, obtendrás automáticamente una sesión interactiva de bash

## 2. En el contenedor anterior con que opciones tiene que haber sido arrancado para poder interactuar con las entradas y salidas del contenedor


## 3.¿Cómo sería un fichero docker-compose para que dos contenedores se comuniquen entre si en una red solo de ellos?


## 4.¿Qué hay que añadir al fichero anterior para que un contenedor tenga la IP fija?


## 5.¿Que comando de consola puedo usar para saber las ips de los contenedores anteriores? Filtra todo lo que puedas la salida.


## 6.¿Cual es la funcionalidad del apartado "ports" en docker compose?


## 7.¿Para que sirve el registro CNAME? Pon un ejemplo


## 8.¿Como puedo hacer para que la configuración de un contenedor DNS no se borre si creo otro contenedor?


## 9. Añade una zona tiendadeelectronica.int en tu docker DNS que tenga

    - www a la IP 172.16.0.1
    - owncloud sea un CNAME de www
    - un registro de texto con el contenido "1234ASDF"
    - Comprueba que todo funciona con el comando "dig"
    - Muestra en los logs que el servicio arranca correctamente

## 10.Realiza el apartado 9 en la máquina virtual con DNS