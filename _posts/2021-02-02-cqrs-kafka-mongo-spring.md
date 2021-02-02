---
layout: post
title:  "Demo CQRS con SpringBoot + Kafka + Mongo"
date:   2021-02-02 18:51:00 +0100
categories: demo
author: Diego Rubio Abujas
tags: node demo kafka springboot
---

![diagrama]({{ site.url }}{{ site.baseurl }}/assets/images/cqrs-kafka-mongo-spring-boot/diagram.jpg)

# Introducción
En esta ocasión vamos a plantear una cosilla con la que llevo peleándome desde antes de navidades. Vamos allá, hace unos meses estuvimos viendo cómo realizar una prueba de concepto de **CQRS** con **Event Sourcing**,se la que hablaré mas abajo. La idea me pareció interesante. Por otro lado estuve viendo dos cursos uno de [Apache Kafka](https://openwebinars.net/cursos/apache-kafka/) y otro de [Mongo](https://openwebinars.net/cursos/apache-kafka/), por lo que también me parecia intersante hacer algun proyectillo de prueba en el que estuviesen presentes estas tecnologías.

## CQRS
La primera cuestión que el lector puede que se plantee es qué demonios es esto de CQRS. Bien CQRS por lo que se es un concepto conocido como *Segregación de Responsabilidad de Consultas y Comandos*. Este patrón fue introducido por **Greg Young** (Os sugiero que busquéis en Youtube conferencias suyas o leáis algún artículo es muy bueno). 

Este patrón nos sugiere que hagamos una separación entre las consultas del dato, de nuestro dominio, lo que se denominan **Consultas** y que son únicamente lecturas. Y por otro lado lo que vienen siendo Agregados (escrituras, modificiones y borrados) en lo que denominaremos **Commandos**. ¿Por qué hacer esta separación? Pensemos en un dominio de dato que tenga un elevadísimo numero de lecturas pero un reducido numero de modificaciones, también pensemos en que queremos tener un historico de todas las modificaciones que se realizan en los datos. Además queremos tener una base de datos optimizada para escrituras y otra optimizada para lecturas. Aparte queremos tener la posibilidad de poder ofrecer unos recursos, en espacio, memoria, contenedores, cpu para las consultar y otro para los comandos. Más o menos esta es un poco la idea que se me quedó de este patrón.

## Event Sourcing
Entonces resumiento: quiero separar el servicio de consultas y el de comandos; y no solo eso, quiero además que tengan bases de datos distintas, puede que con el mismo dato o una versión reducida de éste. Pensémoslo voy a poder hacer frente a un elevado número de consultas, y que las modificaciones,borrados e inserciones no bloqueen el servicio de consultas. 

¿Pero como voy a hacer que las bases de datos estén iguales, consistentemente y con los mismos datos?. Si plantea que cuand ose haga una modificación se realiza una petición al servicio de consulta, estoy otra vez como al principio con el mismo problema. Es ahí donde entra el **Event Sourcing**, o flujo de eventos. La idea es la siguiente en lugar de que el servicio de comandos llame directamente y de forma sincrona al servicio de consulta para que actualice los cambios , vamos a trabajar conun flujo de eventos, que puede ser implementado de muchas maneras, pero en que en nuestro caso tednrá la forma de un *broker de mensajes* con la tecnologia de *Apache Kafka*. 

Que ventajas nos proporciona este patrón, bien basándome en la escasa experiencia que tengo en el tema puedo nombrar dos. 

1. Por un lado vamos a poder hacer frente a un mayor número de peticiones, al disponer de dos servicios podemos dar mas recursos a las lecturas que a las escrituras y modificaciones. Además en caso de haber muchisimas peticiones de lectura, hemos tener en cuenta que las modificaciones que se hagan mediante el broker de mensajes serán asíncronas, y no bloquearán las operaciones de lectura que se estén demandando en el microservicio de consulta en este caso. 

2. La segunda consa que nos proporciona este patrón, es que dado que todos los comandos que se realicen van a ser mensajes del broker, dispondremos de un histórico con todos los cambios que se han ido realizando en el modelo, dominio en el dato en cuestión.

Resumiendo, vamos a a tener dos servicios separados con dos bases de datos separadas. El comando se va a encargar solo de aceptar peticiones para modificar, crear o eliminar. Y va a comunicarse de forma asincrona con el servicio de consulta para que éste refleje estos cambios en su base de datos.

Esto nos plantea una situación un tanto inusual ya que cuando yo por ejemplo creo un nuevo registro, no tengo la garantía de que se cree en el momento, sino de forma asíncrona. Lo explico en el dibujo de más arriba en dos flujos.

### Flujo command (negro) 
1. Realizo una petición al servicio para crear un nuevo Teléfono (Sí, en este caso hablámos de una base de datos de teléfonos).
2. Se inserta un nuevo regitro en la base de datos del microservicio de Command.
3. En el topic del broker de mensajes se envía los datos del evento que ha ocurrido, hemos introducir un nuevo teléfono, y los datos necesarios para que el sericio de query inserte este teléfono en su base de datos.
4. En lo que respecta al usuario, ya le podemos dar el resultado de la operación. Pero no podemos darle un `OK`, ya que si el hace en el momento un `GET` al servicio de query verá que ese dasto aún no existe, es necesario que la operación asíncrona se realice y se efectúen los cambios. No sabemos cuanto va a tardar esto, puede ser segundos o si el servicio de query esta a tope, minutos. Por lo tanto en lugar de decirle OK, le decimos ACCEPTED. Es decir, hemos oido tus peticiones estamos trabajando en ello.
5. En el servicio de consulta hay un Listener escuchando el topic de Kafka, cuando buenamente pueda recibiría el mensaje. He realizado una prueba de concepto con varios contenedores de Query, y un solo topic de kafka, y he pdodido comprobar que solo le llega a uno. 
6. Finalmente una vez el query ha obtenido el mensaje con el evento y `JSON` inserta en la base de datos de Query los cambios realizados quedando ambas bases de datos iguales.
 
### Flujo query (rojo)
De este no hay mucho que decir, se realizan únicamente operaciones de lectura de la base de datos. Y hay que tener en cuentra que puede darse el caso de que se realice una operacion de agregación en el command y aún no se haya visto reflejada en el query por encotnrarse el evento aun pendiente de procesar en el broker de Kafka. 

# Implementando la prueba
Para esta prueba de concepto vamos a plantear una hipotética aplicación de móviles, en la que dispondremos de la siguiente información:

```json
{
    "name": "iphone12",
    "model": "11",
    "color": "red",
    "price": 800.99
}
```

## docker-compose
Es un ejemplo sencillo, por un lado tendremos un endpoint en el *command* para crear nuevos móviles. 
En el query dispondremos de un servicio que dado el nombre de un teléfono devuelva el teléfono en cuestión.

Vamos a disponer de dos *endpoints+ desarrollados con **Spring Boot** que dipondrán de servicios `REST` que realicen estas operaciones. Estos irán en un contenedor con dos puertos expuetos.

También dispondremos de un contenedor con *Apache Kafka* con un topic para gestioanr los eventos. Y un *Apache Zookeper* en otro contenedor.

Dos contenedores con **Mongo**, una para query y otro para command.

A continuación moostramos como hemos planteado el **docker-compose**:

```
version: '3'

services:

  zoo1:
    image: zookeeper:3.4.9
    hostname: zoo1
    ports:
      - "2181:2181"
    networks:
      - kafka_net
    environment:
      ZOO_MY_ID: 1
      ZOO_PORT: 2181
      ZOO_SERVERS: server.1=zoo1:2888:3888
    volumes:
      - ./zk-single-kafka-single/zoo1/data:/data
      - ./zk-single-kafka-single/zoo1/datalog:/datalog

  kafka1:
    image: confluentinc/cp-kafka:5.5.1
    hostname: kafka1
    ports:
      - "29092:29092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: kafka
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka1:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_CREATE_TOPICS: create-phone:2:1
      KAFKA_ZOOKEEPER_CONNECT: "zoo1:2181"
      KAFKA_BROKER_ID: 1
      KAFKA_LOG4J_LOGGERS: "kafka.controller=INFO,kafka.producer.async.DefaultEventHandler=INFO,state.change.logger=INFO"
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    volumes:
      - ./zk-single-kafka-single/kafka1/data:/var/lib/kafka/data
    depends_on:
      - zoo1
    networks:
      - kafka_net
    healthcheck:
      test: [ "CMD", "nc", "-vz", "localhost", "9092" ]
      interval: 30s
      timeout: 10s
      retries: 3

  spring-boot-cqrs:
    build:
      context: .
      dockerfile: Dockerfile
    image: demo-cqrs
    hostname: demo-cqrs
    ports:
      - "8081:8081"
      - "8082:8082"
    depends_on:
      kafka1:
        condition: service_healthy
      mongo-command:
        condition: service_healthy
      mongo-query:
        condition: service_healthy
    networks:
      - kafka_net
      - mongo_net

  mongo-command:
    image: mongo:latest
    container_name: mongo-command
    hostname: mongo-command
    ports:
      - 30000:27017
    volumes:
      - ./data1/db:/data/db
      - ./data1/configdb:/data/configdb
    environment:
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=secret
      - MONGO_DB_NAME=examples
    command: [ --auth , --bind_ip_all, --replSet, rs0 ]
    restart: always
    networks:
      - mongo_net
    healthcheck:
      test: test $$(echo "rs.initiate().ok || rs.status().ok" | mongo -u root -p secret --quiet) -eq 1
      interval: 120s
      start_period: 60s

  mongo-query:
    image: mongo:latest
    container_name: mongo-query
    hostname: mongo-query
    ports:
      - 30001:27017
    volumes:
      - ./data2/db:/data/db
      - ./data2/configdb:/data/configdb
    environment:
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=secret
      - MONGO_DB_NAME=examples
    command: [ --auth , --bind_ip_all, --replSet, rs0 ]
    restart: always
    networks:
      - mongo_net
    healthcheck:
      test: test $$(echo "rs.initiate().ok || rs.status().ok" | mongo -u root -p secret --quiet) -eq 1
      interval: 120s
      start_period: 60s

volumes:
  data01:
    driver: local
  data2:
    driver: local

networks:
  kafka_net:
    driver: bridge
  mongo_net:
    driver: bridge

```

