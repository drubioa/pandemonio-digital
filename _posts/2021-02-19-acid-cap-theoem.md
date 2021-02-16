---
layout: post
title:  "Modelo ACID & Teorema CAP"
date:   2021-02-16 19:02:00 +0100
categories: documentación
tags: databases
author: Diego Rubio Abujas
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/acid-and-cap-theorem/databases.png)

# Introduccióm

En este artículo quiero hablar del modelo ACID y el teorema CAP. La motivación de este artículo es dar un repaso a ambos conceptos, y repasar un poco su relación con las bases de datos de tipo noSql. ¡Vamos allá!

# ACID

Partamos de qué es ACID, bien según nuestra amiga Wikipedia:

> En bases de datos se denomina **ACID** a las características de los parámetros que permiten clasificar las transacciones de los sistemas de gestión de bases de datos. Cuando se dice que una acción es ACID compliant se indica -en diversos grados- que ésta permite realizar transacciones.
En concreto, ACID es un acrónimo en inglés de Atomicity, Consistency, Isolation and Durability: Atomicidad, Consistencia, Aislamiento y Durabilidad, en español.

Y bueno definamos un poco también que una **transacción** es un conjunto de... bueno que lo explique nuestra amiga Wikipedia también:

> Una **transacción** es una interacción con una estructura de datos compleja, compuesta por varios procesos que se han de aplicar uno después del otro. La transacción debe realizarse de una sola vez y sin que la estructura a medio manipular pueda ser alcanzada por el resto del sistema hasta que se hayan finalizado todos sus procesos.

Las propiedades ACID fueron definidas en California, en los años 70.

## Atomicity ó Atomicidad

Cuando una operación o transacción es atómica, tenemos la garantía de que se va a realizar al completo o no se va a realizar, pero no va a quedar incompleta. O "todo o nada".

## Consistency ó Consistencia

Esta propiedad hace referencia a la Integridad de nuestra base de datos, una vez se realice la transacción la base de datos quedará en un estado consistente o integro, no habrá reglas rotas como relaciones foranes que no se cumplan, claves primarias repetidas, nulos donde no debería haber nulos, etc. Es decir, la transacción cumplirá las reglas establecida por nuestra base de datos.

## Isolation ó Aislamiento

Una operación será aislada de otras, es decir una operación o transacción no va a afectar a otras transacciones. Las transacciones sobre una misma información deben de ser independientes.

## Durability ó Durabilidad

Una vez haya terminado la transacción, los cambios perdurarán en el tiempo. 

# No Sql

Las bases de datos No Sql, acabara un amplio número de sistemas gestores de bases de datos pensadas para trabajar de manera distribuida; y entre los que podemos enumerar Mongo, Cassandra, Elastic Search... etc. 

Estas bases de datos, por lo que poco que sé y por lo que he visto tienen varias características, de entre las cuáles quiero enumerar las siguientes.

1. Escalan muy bien de manera horizonal, no tengo que meter mas hardware a mi base de datos única, sino que ampliar el cluster con nuevas máquinas, contenedores, etc.
2. Debido a este ecosistema de muchas máquinas, se crea un sistema replicar, particiones, todo ello hace que los JOIN y las relaciones entre colecciones, tablas se gestionen de una forma bastante poco eficiente, y en algunos casos no se pueda gestionar.
3. Pueden soportar mayor carga de peticiones, y escalar mucho mejor que las bases de datos relacionales.
4. **NO garantizan completamente ACID.**

# Teorema CAP

Y finalmente vamos a hablar de este teorema, que finalmente nos aclara la situación de las bases de datos no sql. Este teorema es una conjetura de Eric Brewer en 2000, que posteriormente fue probado por Seth Gilbert y Nancy Lynch en 2002. Dicho Teorema nos plantea los siguiente:

![]({{ site.url }}{{ site.baseurl }}/assets/images/acid-and-cap-theorem/nosql-triangle.png)

Existen tres propiedades:

1. **Consistencia**:  una lectura va a devolver el resultado mas reciente.
2. **Disponibilidad**: una lectura petición va a recibir una respuesta correcta en un periodo de tiempo razonable, aunque puede no ser la mas reciente.
3. **Tolerancia a particionado:** el sistema sigue funcionando a pesar de que algunos nodos de la red pasen a estar caídos o no disponibles.

El teorema CAP nos dice que **un sistema de base de datos distribuido no puede garantizar estos tres puntos**, únicamente puede garantizarnos dos de ellos. En el gráfico de más arriba vemos diversas bases de datos, y cómo van agrupadas en dos de las propiedad de este teorema.

> El teorema CAP surgió de una suposición del informático Eric Brewer, que la mencionó durante su conferencia en el Simposio sobre Principios de Computación Distribuida (PODC, por sus siglas en inglés) en el año 2000. Por ello, esta propuesta sobre las limitaciones de las características de los sistemas distribuidos también se conoce como teorema de Brewer o Brewers theorem. En 2002, Seth Gilbert y Nancy Lynch, del MIT, demostraron su validez con evidencia axiomática, estableciéndola como teorema

No debemos confundirnos, que una base de datos no garantice una de las tres propiedades no quiere decir que abandone la garantía en todo momento, es solo que en casos excepcionales no puede garantizar el cumplimiento de esa propiedad.