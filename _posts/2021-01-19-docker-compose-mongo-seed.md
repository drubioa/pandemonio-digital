---
layout: post
title:  "Docker Compose + Mongo Seed"
date:   2021-01-29 01:03:00 +0100
categories: demo
author: Diego Rubio Abujas
tags: mongo docker
---

![pair-programming]({{ site.url }}{{ site.baseurl }}/assets/images/docker-compose-mongo-seed/docker-mongo.png)

En esta ocasión vamos a volver a meternos con MongoDB y Docker-Compose, en concreto con una pequeña refriega que he tenido durante el día de hoy. La situación es la siguiente: quiere tener un contenedor de mongo levantado, pero me gustaría que tuviese ya valores incluidos a partir de un fichero tipo `json`. ¿Cómo lo hago?.

# Solución
Bien me alegro que lo preguntes, para ello vamos a definir una solución que es incluir en el `docker-compose.yml` un nuevo servicio al que llamaremos **mongo-seed**. A continuación incluyo un fichero docker-compose de ejemplo:

```yaml
version: '3'

services:

  mongo:
    image: mongo:latest
    hostname: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=secret
    command: [--auth]
    networks:
      - my-network

  mongo-seed:
    build: ./data
    networks:
      - my-network
    depends_on:
      - mongo

networks:
  my-network:
    driver: bridge
```

Bien poco vamos a decir del servicio **mongo** salvo que expone unos puertos, tiene un hostname, un usuario y una clave de administración. También que se le incluye el comando `--auth` y que hemos creado una red que comunica mongo con mongo-seeed.

Nos centramos en `mongo-see` este servicio depende de mongo y se construirá a partir de un `Dockerfile` que paso a indicar a continuación:

```
FROM mongo:latest

COPY init.json /init.json

CMD mongoimport --uri "mongodb://root:secret@mongo:27017/cv?authSource=admin" --collection users --type json --file /init.json --jsonArray
```

De este fichero `Dockerfile` podemos observar en primer lugar que es otra imagen de mongo. Además observamos que va a copiar un fichero json a la imagen que poasteriormente va a utilizar para hacer `mongoimport`; este json será un array de objetos que se insertarán en la colección `users`. Finalmente observamos un comando que lo que hará es conectarse a nuestro contenedor de mongo e importar a partir del json toda la colección `users`.

```json
[
  {
    "name": "Joe Smith",
    "email": "jsmith@gmail.com",
    "age": 40,
    "admin": false
  },
  {
    "name": "Jen Ford",
    "email": "jford@gmail.com",
    "age": 45,
    "admin": true
  }
]
```

Si accedemos a nuestra base de datos mongo mediante el comando:

```bash
mongodb://root:secret@localhost:27017/db?authSource=admin&readPreference=primary&appname=MongoDB%20Compass&ssl=false
```

Podremos observar que se ha creado la tabla con los registros del json.

# Conclusión
Como hemos podido observar, valiéndonos de otra imagen `mongo` y de un json con datos hemos podido "sembrar una semilla" en nuestra base de datos de mongo con una serie de datos para nuestra colección, que en mi caso utilizaré posteriormente para una prueba de concepto que estoy pensando.

Poco mas que aportar en este pequeño post, espero que os a alguien os sea de ayuda.