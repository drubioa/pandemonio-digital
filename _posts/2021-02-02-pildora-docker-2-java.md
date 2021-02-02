---
layout: post
title: "Docker con 2 jars"
date:   2021-02-02 22:27:00 +0100
categories: pildora
author: Diego Rubio Abujas
tags: java docker pildoras
---

![imagen-cabecera]({{ site.url }}{{ site.baseurl }}/assets/images/pildora-docker-2-jars/docker-java.png){:class="page.image"}

En esta ocasión vamos a plantear una pequeña *pildorilla técnica* a raíz de una situación que me he encontrado. Supongamos que para un contendedor de *docker* queremos dejar lanzadas dos microservicios, aplicaciones... en resumidas cuentas queremos lanzar dos *jar* en un mismo contenedor.

Veamos como hemos resuelto este problema con los dos siguientes ficheros:

Por un lado tenemos el *Dockerfile* con dos imágenes. Una con maven para compilar, testear y empaquetar el `jar`. Y otra que lo que hará es coger los dos ficheros `jar` generados movelos a una carpeta y cargar un script de `sh`. Los ejemplos que estuve viendo iban con bash, pero al tratarse de una imagen `alpine` lo hemos realizado con `sh`

```
FROM maven:3.6.3-openjdk-8-slim AS build
COPY demo-command/src /usr/src/demo-cqrs/demo-command/src
COPY demo-command/pom.xml /usr/src/demo-cqrs/demo-command
COPY demo-query/src /usr/src/demo-cqrs/demo-query/src
COPY demo-query/pom.xml /usr/src/demo-cqrs/demo-query
COPY pom.xml /usr/src/demo-cqrs
RUN mvn -f /usr/src/demo-cqrs/pom.xml clean package

FROM openjdk:8-alpine AS demo-cqrs
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring
COPY --from=build /usr/src/demo-cqrs/demo-command/target/demo-command-0.0.1-SNAPSHOT.jar /usr/app/demo-command-0.0.1-SNAPSHOT.jar
COPY --from=build /usr/src/demo-cqrs/demo-query/target/demo-query-0.0.1-SNAPSHOT.jar /usr/app/demo-query-0.0.1-SNAPSHOT.jar
ADD start.sh .
EXPOSE 8081 8082
CMD ["sh", "start.sh"]
```

Hasta aqui poco que decir, la duda es que demonios hace ese `start.sh`. Paso a mostrarlo a continuación:

```sh
#start.sh
java -jar /usr/app/demo-command-0.0.1-SNAPSHOT.jar & java -jar /usr/app/demo-query-0.0.1-SNAPSHOT.jar
```

Como vemos este `script` lo unico que hará es lanzar ambos fichero `jar` con java. Y bueno con todo esto tenemos resuelto un contenedor con las aplicaciones java que queramos en ejecución.