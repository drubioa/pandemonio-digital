---
layout: post
title:  "Demo spring boot + mongodb + docker-compose"
date:   2020-12-29 20:00:00 +0100
categories: demo
author: Diego Rubio Abujas
tags: springboot demo databases docker
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/demo-spring-boot-mongodb-docker-compose/image.png){:class="page.image"}

[Enlace al repositorio de Github][repo-github]

En esta entrada voy a dejar documentada una prueba de concepto que he estado realizando. La idea es sencilla, vamos a montarnos un proyecto que consista en dos contenedores:

1. Un contenedor con un microservicio con **spring boot** que se conecte a una base de datos Mongo DB mediante **Spring Data** y realiza una inserción y una consulta. Las llamadas se harán mediante protocolo REST. Y la idea es que la configuración se la mínima e indispensable.
2. Un contenedor con una base de datos **mongoDB** con un usuario, contraseña y una base de datos y colección inicializadas.

La idea principal de realizar esta prueba viene en gran parte por los recientes cursos que he realizado de docker y mongoDB. En concreto uno que recomiendo que es  [MongoDB for Java Developers](https://university.mongodb.com/courses/M220J/about).

# La Base de datos

Para la base de datos utilizaremos mongo en un contenedor que hemos definido en el docker compose. Cabe destacar que en el mismo docker-compose se definen los usuarios y contraseñas ROOT, el nombre de la base de datos.

```yaml
version: '3'

services:
  app:
    build:
      context: ./
      dockerfile: Dockerfile
    image: app
    hostname: app
    ports:
      - "8081:8081"
    depends_on:
      - mongodb
    networks:
      - my-network

  mongodb:
    image: mongo:latest
    container_name: "mongodb"
    hostname: mongodb-host
    environment:
      - MONGO_INITDB_ROOT_USERNAME=mongoadmin
      - MONGO_INITDB_ROOT_PASSWORD=secret
      - MONGO_INITDB_DATABASE=example
    ports:
      - "27017:27017"
    networks:
      - my-network
    volumes:
      - ./data/init-mongo.js:/docker-entrypoint-initdb.d/mongo-init.js:ro

networks:
  my-network:
    driver: bridge
```

Además como se puede observar se incluye ademas un enlace a un archivo js en el que se inicializa la base de datos. Ahi podemos definir usuarios, colecciones, indices... etc. 

```javascript
db.createUser(
  {
    user: 'api_user',
    pwd: 'NdEep0XLpMNKUmgQVa81oDCx7mrSRodh0Z79qdX3',
    roles: [{ role: 'readWrite', db: 'example' }],
  },
);
db.createCollection('person');

```

También importante dar un nombre al host donde estará la base de datos, y configurar una red que comunique el contenedor en el que se encuentra la base de datos con el contenedor de la app. 

Para probar si el contenedor de la base de datos arranca correctamente podemos hacerlo mediante [MongoDB Compass](https://www.mongodb.com/products/compass).

# Microservicio Spring Boot

La app consta básicamente de las siguientes clases:

![Estructura de proyecto]({{ site.url }}{{ site.baseurl }}/assets/images/demo-spring-boot-mongodb-docker-compose/intellij-capture.png){:class="page.image"}

- Un controlador con dos endpoint un GET y un POST.
- Un service con dos métodos uno para buscar una Person por nif, y otro para insertar una nueva Person en la base de datos.
- Un mapper para transformar el modelo al dto y viceversa.
- Un model y un dto
- Una clase para lanzar un exception not found cuando no exista el Person con el dni.
- Un repository con la colección de mongo.
- El application con la configuración de mongo

# Configuración de Mongo en Spring Boot
Por lo que he estado viendo mientras realizaba esta prueba de concepto, la conexión con la base de datos de mongo se puede realizar de varias formas.

1. Nosotros la hemos realizado directamente en una uri en el **application.yml**.

```yaml
server:
  port: 8081
spring:
  data:
    mongodb:
      uri: mongodb://api_user:NdEep0XLpMNKUmgQVa81oDCx7mrSRodh0Z79qdX3@mongodb-host:27017/example
```

2. En lugar de en la uri se puede indicar en varios campos como vemos en el siguiente ejemplo.

```
    spring.data.mongodb.authentication-database=admin
    spring.data.mongodb.username=root
    spring.data.mongodb.password=root
    spring.data.mongodb.database=test_db
    spring.data.mongodb.port=27017
    spring.data.mongodb.host=localhost
```

También es posible definir toda la configuración en una clase con *@Configuration*  tipo **MongoConfig.java**. Ahi estaría bien hacer uso de *@Value* que hicieran referencias al properties .

```java
@Configuration
public class SimpleMongoConfig {
 
    @Bean
    public MongoClient mongo() {
        ConnectionString connectionString = new ConnectionString("mongodb://localhost:27017/test");
        MongoClientSettings mongoClientSettings = MongoClientSettings.builder()
          .applyConnectionString(connectionString)
          .build();
        
        return MongoClients.create(mongoClientSettings);
    }

    @Bean
    public MongoTemplate mongoTemplate() throws Exception {
        return new MongoTemplate(mongo(), "test");
    }
}
```

# Dockerfile
 Como viene siendo habitualmente últimamente en todos los proyectos que dockerizo, se utiliza un contenedor con un maven para construir el paquete jar, y otro con open-jdk para su despliegue.

 ```
    FROM maven:3.6.3-openjdk-8-slim AS build
    COPY src /usr/src/app/src
    COPY pom.xml /usr/src/app
    RUN mvn -f /usr/src/app/pom.xml clean package

    FROM openjdk:8-alpine AS app
    RUN addgroup -S spring && adduser -S spring -G spring
    USER spring:spring
    COPY --from=build /usr/src/app/target/demo-0.0.1-SNAPSHOT.jar /usr/app/demo-0.0.1-SNAPSHOT.jar
    EXPOSE 8081
    ENTRYPOINT ["java","-jar","/usr/app/demo-0.0.1-SNAPSHOT.jar"]

 ```

# Readme
 Otra cosilla que he aplicado en esta prueba de concepto es intentar mejorar un poco la calidad de los ficheros README.md que aparecen en el proyecto. Basándome en otros ejemplos que he visto he incluido soporte para idiomas (incluyendo dos ficheros y enlazándolos arriba del todo) y he visto que se pueden incluir iconos de forma nativa en el README.md (como se puede ver en los títulos).

# Probando el servicio
La prueba es la siguiente:

1. Se realiza una petición POST para insertar un nuevo registro. Se comprueba que se devuelve 200.
2. Se realiza una petición GET para comprobar que el registro se ha guardado correctamente en la base de datos.

En el fichero README se encuentra mas información sobre como lanzar, probar este proyecto. Así como una colección de pruebas de Postman que es posible importar. 



Para orquestar la construcción y despliegue haremos uso de **docker-compose**. 


[repo-github]: https://github.com/drubioa/demo-mongo-springboot