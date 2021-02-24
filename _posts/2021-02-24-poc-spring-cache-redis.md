---
layout: post
title:  "Spring Cache + Redis"
date:   2021-02-24 23:30:00 +0100
categories: demo
tags: poc springboot
author: Diego Rubio Abujas
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/poc-spring-cache-redis/superior.png)

[Enlace repositorio de github](https://github.com/drubioa/demo-spring-cache-redis)

En esta ocasión nos metemos a hablar un poco sobre **Spring Cache** y los distintos tipos de caches que podemos incluir en nuestra aplicación. Esta problemática ya la he visto en varios proyectos, disponemos de una Servicio que realiza una operación que tarda en realizarse, por ejemplo puede que tenga llamar a varios servicios, hacer consultas pesadas, cálculos, etc... y resulta que es una operación que se realiza mucho por parte de nuestros usuarios y cuyos resultados no cambian en el tiempo. Mi primer encuentro con Spring Cache fue en un proyecto de banca en el cual había que obtener los movimientos de un cliente, y estas solo se actualizaban cada 2 o 3 días, y encima el usuario estaba constantemente pidiéndolas si entraba en la aplicación varias veces al día. 

Bien la solución ante este tipo de situaciones es utilizar una caché: la primera vez que se tiene que realizar esta operación almacenamos en una memoria (una caché) el resultado y le asignamos un identificado único que suele estar asociado a la petición que nos ha resultado este usuario. De esta forma, cuando el usuario vuelva a realizar la petición primero comprobaremos si ya existe un resultado a esta petición en nuestra memoria cache y de ser así en lugar de volver a realizar la operación directamente devolvemos el resultado de esta búsqueda en nuestra memoria caché. 

¿Qué es una caché? Bien, una caché es una capa de almacenamiento de alta velocidad en la que almacenamos de manera temporal información.

## Spring Caché

En el siguiente [enlace](https://spring.io/guides/gs/caching/) disponemos de un ejemplo de cómo configurar una aplicación simple con la caché que nos proporciona Spring, este ejemplo sencilla funciona con dos:

1. @Cacheable: esta anotación sobre un método nos permitirá indica que se caché lo que devuelva este método; de forma que si se vuelve a realizar una llamada a este método en lugar de llevarse a cabo la operación se tira de la memoria y se devuelve el objeto.
2. @EnableCaching: Esta anotación se indica donde tengamos el Application, e indica a Spring que habilite la caché al arrancar. Al incluir esta anotación se configura automáticamente la caché, podemos especificar mas configuraciones en el yml.

Hay una documentación muy amplia en la [web de Spring](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#cache).

La Caché al igual que @Transactional está pensaba en Spring para que suponga el menor código posible como podemos ver. 

### Tipos de Caché

Según  [javapoint](https://www.javatpoint.com/spring-boot-caching) podríamos decir que hay cuatro tipos de cachés:

1. Cache en memoria: 

> In-memory caching increases the performance of the application. It is the area that is frequently used. **Memcached** and **Redis** are examples of in-memory caching. It stores key-value between application and database. Redis is an in-memory, distributed, and advanced caching tool that allows backup and restore facility. We can manage cache in distributed clusters, also.

1. Base de datos

> Database caching is a mechanism that generates web pages on-demand (dynamically) by fetching the data from the database. It is used in a multi-tier environment that involved clients, web-application server, and database. It improves scalability and performance by distributing a query workload. The most popular database caching is the first level cache of Hibernate.

1. Web server

> Web server caching is a mechanism that stores data for reuse. For example, a copy of a web page served by a web server. It is cached for the first time when a user visits the page. If the user requests the same next time, the cache serves a copy of the page. It avoids server form getting overloaded. Web server caching enhances the page delivery speed and reduces the work to be done by the backend server.

1. CDN Caching

> The CDN stands for Content Delivery Network. It is a component used in modern web applications. It improves the delivery of the content by replicating commonly requested files (such as HTML Pages, stylesheet, JavaScript, images, videos, etc.) across a globally distributed set of caching servers.
It is the reason CDN becomes more popular. The CDN reduces the load on an application origin and improves the user experience. It delivers a local copy of the content from a nearby cache edge (a cache server that is closer to the end-user), or a Point of Presence (PoP).

Por otra parte consultando una [entrada de paradigma](https://www.paradigmadigital.com/dev/caches-por-donde-empezar/) se nos indica que hay dos tipos de cache:

Tenemos la **caché local** o de proceso, esta caché pertenece a la aplicación que requiere de utilizar una caché y asigna un espacio dinámico de memoria.

En general vamos a querer que nuestra aplicación pueda escalar en varios contenedores de forma que para ello necesitaremos de una **cache distribuid**a que forme una gran caché lógica.

### Recorridos por Cachés Distribuidas

Vamos a mencionar algunas de las cachés distribuidas más conocidas y algunas cosillas curiosas que nos puede proporcionar, en la prueba de concepto que hemos realizado hemos utilizado **Redis**, pero se podría haber hecho con cualquiera de las que vamos a citar.

1. **Redis**: esta caché es un sistema de almacenamiento en memoria de código abierto, además de una caché se puede utilizar como memoria o incluso como broker de mensaje. Os recomiendo que os paséis por su [web](https://redis.io/) y le echéis un vistazo a su tutorial interactivo.
2. **Hazelcasts**: es un DataGrid de memoria distribuido hecho en java, también de código abierto. Está pensado para un clúster de computadoras y trabajar con escalabilidad horizontal.
3. **MongoDb** In Memory: me ha sorprendido ver que mongo Tambien dispone de un almacenamiento en memoria de alta disponibilidad que posibilita utilizar una caché en formato de Documentos Bson.
4. Otras que podemos citar y que no conozco demasiado son : *Couchbase*, *Memcached* y *Ehcache*. Son muy utilizadas y también merecería la pena indagar mas en cómo funcionan y qué beneficios aportan.

### Cache Vs Buffer

Y a raíz de lo que nos plantea en la web de Spring nos planteamos la siguiente pregunta: ¿Qué diferencias hay entre la caché y el buffer?. Vamos a poner la tabla que se nos plantea en  [javapoint](https://www.javatpoint.com/spring-boot-caching) :

| Caché | Buffer |
| --- | --- |
| LRU: Basada en uso mas reciente | FIFO: Primero en entrar primero en salir |
| Su tamaño viene determinado por el tamaño de la página de caché | Su tamaño viene determinado por el tamaño de bloque de memoria IO del buffer |
| Periodo de vida muy largo | Periodo de memoria muy corto | 
| Otimizado para lecturas | Optimizado para escrituras |

# Prueba de Concepto

Vamos a realizar una prueba de concepto en la que trabajaremos dos de los temas de los que hemos hablado anteriormente. 

Por un lado vamos a disponer de un microservicio desarrollado con spring boot que dispondrá de un servicio que tendrá una operación costosa que tardara varios segundos en realizarse. Este método estará cacheado mediante la anotación cacheable:

```java
    @Cacheable(value="example", key="#n")
    public Long example(Long n) {

        // Example of heavy service wait 5 seconds after return value
        try {
            Thread.sleep(waitSeconds * 1000);
            log.info("return value after {} seconds", waitSeconds);
            return n;
        } catch (Exception e) {
            log.error(e);
            throw new RuntimeException(e);
        }
    }
```

Al incluir la anotación @Cacheable indicamos que cuando se llame a este método antes de ralizar la operacion Spring comprobará si ya existe una cache de tipo example con el valor de ejemplo que estamos utilizando.

### Configuración

Para configurar nuestra caché redis hemos tenido que incluir la siguiente configuración en nuestro yaml:

```yaml
	 spring:
    cache:
        type: redis
    redis:
        host: redis
        port: 6379
```

Además hemos incluido la anotacion @EnableCaching en nuestra clase Application.

### Dependencias

Ha sido necesario incluir las siguientes dependencias en nuestro pom.xml para poder utilizar redis.

```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

## Docker-compose

Vamos a desplegar varios contenedores para esta prueba, por un lado desplegaremos un contenedor en el que irá nuestro redis exponiendo un puerto y conectado a una red que comunique con otros dos contenedores. 

Estos dos contenedores serán dos despliegues de nuestro microservicio 

A continuación indico cómo ha quedado el `docker-compose:`

```jsx
version: '3'

services:

  redis:
    image: library/redis:6.0.10-alpine
    hostname: redis
    ports:
      - 6369:6369
    networks:
      - my_network

  demo-service-1:
    build:
      context: ./
      dockerfile: Dockerfile
    image: app
    hostname: app
    ports:
      - "8080:8080"
    depends_on:
      - redis
    networks:
      - my_network

  demo-service-2:
    build:
      context: ./
      dockerfile: Dockerfile
    image: app
    hostname: app
    ports:
      - "8081:8080"
    depends_on:
      - redis
    networks:
      - my_network

networks:
  my_network:
    driver: bridge
```

## La prueba

Para probar nuestra aplicacion haremos `docker-compose up` y una vez esten todos los contendores arrancados realizaremos la siguiente prueba y observaremos los resultados de esta:

1. Si realizamos `curl localhost:8080/example/1` tras una espera de un poco más de 5 segundos nos devuelve `200 OK`.
2. Si volvemos a realizar la llamada `curl http://localhost:8080/example/1` observamos que la respuesta es inmediata, ya que está tirando de la caché.
3. Si en lugar de 1 llamamos a `curl http://localhost:8080/example/2`  volvemos a observar esos 5 segundos de espera.
4. Y si ahora en lugar de llamar al contenedor que tenemos desplegado en el puerto 8080 llamamos al  `curl http://localhost:8080/example/2` observamos para nuestro asombra que la respuesta es también inmediato. Y es que al estar distribuida cualquier contenedor configurado para apuntar a nuestro redis hará uso de esta caché.