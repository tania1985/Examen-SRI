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
- Puedes utilizar el siguiente comando de consola para obtener las direcciones IP de los contenedores definidos en tu archivo docker-compose.yml:
        `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -q)`
- Este comando utiliza docker ps -q para obtener solo los IDs de los contenedores en ejecución y luego utiliza docker inspect para obtener y formatear las direcciones IP de cada contenedor.

- Si estás trabajando con un solo contenedor y no con Docker Compose, simplemente sustituye $(docker ps -q) con el ID o el nombre del contenedor que estás inspeccionando.

## 6.¿Cual es la funcionalidad del apartado "ports" en docker compose?

 - El apartado "ports" en un archivo de Docker Compose se utiliza para especificar y mapear los puertos entre el host y el contenedor. Permite exponer y redirigir puertos desde el contenedor hacia el host o entre contenedores.

> Aquí tienes un ejemplo básico de cómo se utiliza el apartado "ports" en un archivo docker-compose.yml:
     ` version: '3'

        services:
            servicio1:
            image: imagen_servicio1
        ports:
            - "8080:80"
`
- El apartado "ports" especifica la asignación de puertos entre el host y el contenedor. En este caso, el puerto 80 del contenedor se mapea al puerto 8080 del host. El formato es "puerto-del-host:puerto-del-contenedor".

- Esto significa que puedes acceder al servicio dentro del contenedor a través del puerto 8080 en el host. Si el servicio dentro del contenedor escucha en el puerto 80, el mapeo de puertos permite que el tráfico enviado al puerto 8080 del host se redirija al puerto 80 del contenedor.

- Puedes especificar múltiples mapeos de puertos para un servicio si es necesario. Además, si no especificas el puerto del host (por ejemplo, "8080"), Docker Compose asignará automáticamente un puerto disponible en el host.

- Este mapeo de puertos es útil cuando necesitas exponer servicios de contenedores al mundo exterior o cuando estás conectando varios servicios a través de la red de Docker Compose.

## 7.¿Para que sirve el registro CNAME? Pon un ejemplo
- El registro CNAME (Canonical Name) en DNS (Sistema de Nombres de Dominio) se utiliza para establecer alias o nombres canónicos para un dominio. Básicamente, un registro CNAME asocia un nombre de dominio con otro nombre de dominio, permitiendo que una máquina sea conocida por más de un nombre.

> Ejemplo:

- Supongamos que tienes un sitio web llamado www.ejemplo.com, y también tienes una versión móvil de tu sitio que quieres que sea accesible a través de mobil.ejemplo.com. En lugar de configurar y mantener dos direcciones IP diferentes para estos dos nombres, puedes utilizar un registro CNAME.

- El registro CNAME se vería algo así:
`mobil.ejemplo.com. IN CNAME www.ejemplo.com.`
- Esto significa que mobil.ejemplo.com es un alias de www.ejemplo.com. Cuando alguien intenta acceder a mobil.ejemplo.com, el servidor DNS redirige la solicitud a la dirección IP asociada con www.ejemplo.com.

- En resumen, el registro CNAME es útil cuando deseas que múltiples nombres de dominio se resuelvan a la misma dirección IP. Esto facilita la gestión y el mantenimiento, ya que si la dirección IP cambia, solo necesitas actualizar un solo registro en lugar de varios. Además, simplifica la administración de servicios que pueden tener múltiples nombres de dominio asociados.


## 8.¿Como puedo hacer para que la configuración de un contenedor DNS no se borre si creo otro contenedor?


## 9. Añade una zona tiendadeelectronica.int en tu docker DNS que tenga

    - www a la IP 172.16.0.1
    - owncloud sea un CNAME de www
    - un registro de texto con el contenido "1234ASDF"
    - Comprueba que todo funciona con el comando "dig"
    - Muestra en los logs que el servicio arranca correctamente

## 10.Realiza el apartado 9 en la máquina virtual con DNS