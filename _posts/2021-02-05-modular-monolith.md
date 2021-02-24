---
layout: post
title:  "Monolitos modulares"
date:   2021-02-05 03:00:00 +0100
categories: arquitectura
tags: arquitectura
author: Diego Rubio Abujas
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/modular-monolith/what-is-monolith.jpg)

La idea de escribir este post surge de un [video que ví en Youtube](https://www.youtube.com/watch?v=_Maknc_e1-g) de un [canal](https://www.youtube.com/channel/UCs5ccxrTx9k8DN9YXnHm5AQ) que sigo sobre Arquitectura de Software, y que os recomiendo encarecidamente, de un arquitecto de software llamado [Manuel Zapata](https://manuelzapata.co/category/arquitectura-de-software/). Además hacía tiempo que quería dejar plasmadas todas mis ideas y opiniones sobre este tema.

Hasta ahora hemos tenido dos modelos arquitectónicos dominantes en lo que a *backend* se refería: **monolitos** y **microservicios**.

# Monolitos
Este es el modelo arquitectónico mas convencional orientado a que todos los servicios estén en un sólo contenedor. Disponemos básicamente de una aplicación que se encarga de ofrecer todos los endpoints, servicios y realizar todos los accesos a la base de datos. 

Si seguimos una estructura *MVC* (Modelo, vista controlador), este tendrá sus clases separados en paquetes para cada parte de la estructura. 

## Ventajas
La **ventaja** de esta arquitectura es que disponemos de toda las funcionalidades de nuestra aplicación en un único proyecto. 

## Desventajas
Su principal **inconveniente** es precisamente ese, si el proyecto engorda mucho en funcionalidades se vuelve farragoso. Y explico en qué sentido, por un lado vamos a tener muchísimas clases, el último monolito en el que estuve trabajando creo que tenia mas de 200 clases. El hecho de tener tantas clases hace que por un lado sea mas complicado de mantener, puede darse el caso de que haya funcionalidades que dependan unas de otras, cualquier cambio puede afectar a otras partes, se dificulta muchísimo el testeo, tarda muchísimo más en compilar y desplegar. En resumen es mucho menos *mantenible*. Es más costoso de construir y desplegar, imaginemos que de todas las funcionalidades que tiene mi monolito hay una que consume muchos recursos o que tiene muchísimas peticiones, pues si quiero desplegar otro contenedor solo por la demanda o requerimientos de esa funcionalidad, toma otro enorme monolito desplegado solo para eso.

# Microservicios
Como solución a esta problemática surgen los microservicios. La idea es la siguiente, en lugar de aglutinar todas las funcionalidades en una sola aplicación vamos desglosar cada funcionalidad, entidad en una aplicación independiente que exponga sus servicios y que no comparta recursos con otros microservicios. De esta forma, cuando un microservicio requiere de alguna funcionalidad de otro se comunica con este.

## Ventajas
Las **ventajas** que nos proporciona esta arquitectura son muchísimas. Se resuelven los inconvenientes planteados por la arquitectura monolítica. Por un lado dado que cada microservicio tiene las funcionalidades relacionadas con una entidad quedan aplicaciones mas pequeñas, centradas en una parte concreta de la lógica de negocio. Éstas son mucho más mantenibles, testeables, rápidas de desarrollar. Aunque no lo he visto en la práctica, esta arquitectura nos brinda la oportunidad de trabajar con distintas tecnologías: es decir, podríamos tener por ejemplos ciertos microservicios hechos con Java (Spring Boot), otros con node, otros golang, python... y un enorme etc. Total, con que los servicios expuestos sean REST y se tenga un marco común de comunicación funcionaría. Además, podríamos tener varios sistemas de base de datos: unos microservicios podrían guardar información en un mongo, otros en un mysql, otros un cassandra... y se nuevo un larguísimo etc.

Y ya que estamos, si tenemos que organizar múchos equipos de desarrolladores o incluso múchos desarrolladores dentro de un mismo *squad* unos podrían trabajar en un microservicio y otros en otros, facilita mucho la organización del trabajo.

El hecho de tener muchas aplicaciones pequeñas en lugar de una enorme, hace que a la hora de desplegar contenedores, jars, wars o lo que sea podamos desplegarlos más rápido y en números distintos. Por ejemplo un microservicio que tenga mucha demanda por parte los usuarios podría tener muchos pods o contenedores desplegados con muchos recursos asignados, mientras otros menos utilizados podrían tener menos. Y ya no solo eso, sino que sería necesario parar toda la aplicación para realizar un despliegue de un cambio, sino solo parte de ella; de igual manera, en caso de error no fallaría toda la aplicación sino solo parte de ella (la parte del microservicio).


## Deventajas
Y como no todo iban a ser ventajas, vamos con las **deventajas** que veo. Por un lado a nivel de arquitectura, no es fácil definir qué es y qué no es un microservicio cuando se nos plantea cómo debemos estructurar las aplicaciones. Pensamos en ejemplos sencillos, pero la realidad es que muchas veces hay que dar muchas vueltas, y aunque se plantee bien quedan microservicios cojos u otros que pasan a ser lo que se denominan **microlitos** (microservicios + monolitos) que son al final monolitos ya que agrupan muchas funcionalidades y rompen el esquema arquitectónico. Otras veces ocurre que se hila demasiado fino, y quedan microservicios absurdamente ridículos, que solo hacen una función, que solo consultan una simple tabla maestra con 5 registros, etc. Como digo es muy difícil, plantear una buena arquitectura de microservicio es muy fácil cagarla, y una vez dejado el regalito es más difícil aún rehacerlo bien. Ocurre un poco como cuando se diseña mal una base de datos relacional, una vez ya esta poblada de datos es muuuuy complicado rehacerlo bien.

Orquestar todos estos microservicios, se puede hacer también un auténtico dolor de cabeza. Y no solo eso, sino monitorizar que todo vaya bien, detectar errores, consultar los logs. Por suerte hay muchas herramientas y soluciones para ello. Pero aún así, es mucho mas complicado gestionar y administrar una mediante microservicios que mediante un monolito.

Otra desventaja que he visto es el tema de los estados, en concreto uno que vi al principio de trabajar es el tema de la *transaccionalidad*. Supongamos que tengo una operación que realiza varios cambios en mi base de datos, y si algo falla debe deshacer todos los cambios. En el monolito tiene una solución que es si algo falla hacer un *rollback*^. Pero en esta arquitectura no es tan sencillo, no tenemos una sola base de datos sino varias, no es un servicio el que va a hacer la modificacion sino *n* servicios. ¿Qué ocurre si alguno de esos servicios falla?¿Cómo deshacemos los cambios?¿Hay marcha atrás?. La respuesta es si, pero no es tan sencilla ni intuitiva como en el caso del monolito, existe el [patrón SAGAS](https://microservices.io/patterns/data/saga.html) con sus orquestadores y coreografías que nos plantea como solucionar esta situación, pero ya digo no es ni por asomo tan sencilla como la planteada por el monolito.

# Monolitos modulares (Monolito + Modular)
>Si no puedes hacer bien un monolito, no esperes hacer bien microservicios
>
> -- Simon Brown

Y ahora el meollo de la cuestión, hemos visto ventajas e inconvenientes de ambas arquitecturas. Ahora vamos a plantear un término medio, un monolito que esté preparado para que si en algún momento hiciera falta extraer módulos en microservicios, es decir un monolito modular.

La idea básica es la siguiente: 

>The idea of a modular monolith is to preserve the idea of encapsulation from above but deploy it in a different manner. We don’t have to deploy a module as a service until we need to scale it. Instead, modules can be deployed as libraries, plugins, namespaces etc., resulting into one file that can be very easily deployed, monitored and debugged. As long as we follow a good modular structure inside the monolith, we can easily extract a service.
>
> https://mozaicworks.com/

Bien dejada esta definición lo explico como yo lo he entendido, vamos a tener nuestro monolito por capas como siempre, pero vamos a estructurarlo en módulos, estos módulos van a ser independientes, podrían inclusos tirar de distintos repositorios. De esta forma, aunque el resultado es una única aplicación, si en algún momento queremos podemos extraer esta funcionalidad de manera limpia a un microservicio. Sin que haya graves problemas de dependencias entre los distintos servicios, y minimizando el impacto de extraer una funcionalidad fuera de la aplicación. 

![diagram]({{ site.url }}{{ site.baseurl }}/assets/images/modular-monolith/diagrama.jpg)

Así pues, por un lado vamos a dejar una estructura de paquetes en la que se van a separar las distintas entidades o funcionalidades de nuestro monolito en módulos, pensados para si en algún momento se requiera se puedan extraer. Estos módulos son independientes, no van depender de otros módulos. Pero, ¿y si necesito comunicarme con otro módulo al igual que se comunican varios microservicios?. En este [artículo](http://www.kamilgrzybek.com/design/modular-monolith-primer/) de Kamil Grzybek se nos plantea varias soluciones a esta problemática, básicamente por lo que entiendo se creará una figura a la que él denomina *Mediador* o bien una *interface* que comunicará un módulo con otro cuando sea necesario que un modulo utilice funcionalidades de otro, de esta forma se simula lo que en microservicios sería o bien una llamada de un microservicio a otro, o bien por ejemplo un broker de mensajes para eventos. De todas formas, tengo pendiente realizar una profunda pruebe de concepto de todo esto. 

Resumiendo, esta arquitectura nos plantea estructurar nuestro monolito para curarnos en salud en caso de que en algún momento una funcionalidad sea candidata a salir del monolito y volar libre como un microservicio. Al final, lo que nos quedaría es una arquitectura mixta en la que nuestro monolito modular conviviría con diversos microservicios llegados el caso. Todo esto me parece una idea muy interesante en determinadas circunstancias.

Dejo el enlace mas abajo a un repositorio de un proyecto estructurado de esta forma.

[Repositorio de ejemplo de Andrei Ganichev](https://github.com/kgrzybek/modular-monolith-with-ddd)
