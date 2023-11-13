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
    `ENTRYPOINT ["/bin/bash"]`
    - Luego, al ejecutar el contenedor, obtendrás automáticamente una sesión interactiva de bash

## 2. En el contenedor anterior con que opciones tiene que haber sido arrancado para poder interactuar con las entradas y salidas del contenedor

    1. Opción -i (interactive):

    - Esta opción mantiene la entrada estándar abierta incluso si no está conectada, lo que permite la interactividad.
      > Ejemplo:
        `docker run -i nombre_del_contenedor /bin/bash`
    2. Opción -t (tty):

    - Esta opción asigna un pseudo terminal (TTY) al contenedor. Es necesario para tener un shell interactivo.
      > Ejemplo:
            `docker run -it nombre_del_contenedor /bin/bash`
    3.Opción --rm:

    - Esta opción elimina automáticamente el contenedor después de que se detiene, lo que es útil para limpiar recursos temporales.
        > Ejemplo:
            `docker run -it --rm nombre_del_contenedor /bin/bash`

## 3.¿Cómo sería un fichero docker-compose para que dos contenedores se comuniquen entre si en una red solo de ellos?

- Para que dos contenedores se comuniquen entre sí en una red dedicada, puedes utilizar Docker Compose para definir la configuración de tus servicios y la red asociada. Aquí tienes un ejemplo de un archivo docker-compose.yml para dos contenedores que se comunican en una red específica:

    ` version: '3'

services:
  servicio1:
    image: imagen_servicio1
    networks:
      - mi_red

  servicio2:
    image: imagen_servicio2
    networks:
      - mi_red

networks:
  mi_red:
    driver: bridge
`
- Explicación:

    version: '3': Indica la versión de la sintaxis de Docker Compose que se está utilizando.

    services: Define los servicios que se ejecutarán como contenedores.

        servicio1 y servicio2: Son los nombres de los servicios. Puedes reemplazarlos con los nombres de tus servicios reales.

            image: Especifica la imagen del contenedor que se utilizará para el servicio. Reemplaza imagen_servicio1 e imagen_servicio2 con las imágenes que deseas utilizar.

            networks: Asocia el servicio a una o más redes. En este caso, ambos servicios están conectados a la red llamada mi_red.

    networks: Define las redes que utilizarán los servicios.

        mi_red: Es el nombre de la red. Puedes cambiarlo según tus preferencias.
            driver: bridge: Especifica el controlador de red que se utilizará. En este caso, se utiliza el controlador de puente, que es común para redes locales de contenedores.
    - Para ejecutar los contenedores con Docker Compose, guarda este archivo como docker-compose.yml y ejecuta el siguiente comando en el mismo directorio donde se encuentra el archivo:
        `docker-compose up`

## 4.¿Qué hay que añadir al fichero anterior para que un contenedor tenga la IP fija?

- En Docker Compose, puedes asignar una dirección IP específica a un contenedor utilizando la opción ipv4_address en la configuración del servicio. Aquí hay un ejemplo de cómo puedes modificar el archivo docker-compose.yml para asignar una dirección IP fija a un contenedor:
version: '3'

` services:
  servicio1:
    image: imagen_servicio1
    networks:
      mi_red:
        ipv4_address: 172.18.0.2  # Puedes cambiar la dirección IP según tus necesidades

  servicio2:
    image: imagen_servicio2
    networks:
      mi_red:
        ipv4_address: 172.18.0.3  # Puedes cambiar la dirección IP según tus necesidades

networks:
  mi_red:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 172.18.0.0/16
`

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