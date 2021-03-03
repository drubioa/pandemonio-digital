---
layout: post
title:  "Mutation testing"
date:   2021-03-03 15:00:00 +0100
categories: testing
author: Diego Rubio Abujas
tags: testing
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/mutation-testing/cover.png)

> "Las pruebas unitarias también son código fuente"

> "Mi código funciona perfectamente porque hago pruebas unitarias"

En esta ocasión abordamos el tema de *mutation testing* o pruebas de mutación, este tema ya surgió hace un par de años hablando con un compañero sobre pruebas unitarias y lo fácil que era engañarlas para cumplir la cobertura que nos pedía Sonar, ha vuelto hace poco a partir de una conversación sobre virtudes y bondades de las pruebas unitarias. Así que me he decidido a recabar un poco de información sobre el tema y dejarlo plasmado en este artículo.

Las pruebas de mutaciones nos permiten evaluar la calidad de nuestros test. Para ello lo que hacen es, partiendo de una prueba unitarias, realizar "pequeñas variaciones" en el la clase que estamos testeando, y generar lo que denominan "un mutante". Cada versión cambiada es un mutante y las pruebas deben ser capaces de distinguir el programa original del mutante a partir del comportamiento observado. A esto se le llama matar al mutante. Una vez se han evaluado todos los tipos de mutaciones se utilizan los mutantes que hayan sobrevivido para establecer una puntuación de mutaciones, y ver si es factible y necesario realizar nuevos test entre nuestras pruebas. Si lo pensamos, es como testear los test, es decir probar si nuestros test unitarios son realmente útiles o no. 

Del resultado de las pruebas de mutación obtenemos una puntuación que cuanto mayor sea mejor serán nuestros test unitarios.

> Mutation Score = (Killed Mutants / Total number of Mutants) * 100

# Tipos de mutaciones

Básicamente podemos enumerar los siguientes tipos de mutaciones:

1. Mutación de estado: eliminación o duplicación de determinadas líneas de código
2. Mutación de valor: se cambia un valor
3. Mutación de operador: se cambia un operador por otro, un + por - por ejemplo
4. Mutación de decisión: una estructura de control, un if por ejemplo, es alterado.

# Ventajas

Algunas de las ventajas que podemos enumerar de las pruebas de mutación son las siguientes:

1. Nos permite evaluar la cobertura "real" de nuestro código.
2. Nos permite identificar nuevas pruebas que realizar.
3. Mejora la detección de errores en el desarrollo de softwre.
4. Permite detectar ambigüedades y fallos en el código.

# Desventajas

Podemos considerar también algunas desventajas:

1. Son muy costosos de generar y ejecutar este tipo de pruebas, ya que implica repetir las pruebas un elevado número de veces. 
2. Requiere tener acceso al código fuente, no se puede aplicar para pruebas de caja negra.
3. Es necesario evaluar detalladamente los mutantes que hayan sobrevivido para ver cómo podemos mejorar nuestra batería de test.
4. Requiere de una automatización para poder generar todos los mutantes.

# Herramientas de automatización

## [Pitest](http://pitest.org/)

![pitest-logo]({{ site.url }}{{ site.baseurl }}/assets/images/mutation-testing/pitest.png)

Esta herramienta está pensada para pruebas en Java. Se puede configurar por plugin de maven, ant o por linea de comandos. Se puede integrar además con Gradle, Eclipse, Intellij y otros IDEs. 

Esta herramienta nos permite configurar muchos operadores de mutación para poder personalizar la generación de mutantes en este ejemplo que nos muestran en su web, si por ejemplo le indicaramos que sobre el código:

```java
if ( i >= 0 ) {
    return "foo";
} else {
    return "bar";
```

Generar un mutante siguiendo CONDITIONALS_BOUNDARY_MUTATOR nos generaría por ejemplo un mutante tal y como el que indicamos a continuación:

```java
if ( i > 0 ) {
    return "foo";
} else {
    return "bar"
```

Si realizamos la automatización de test con el plugin de maven podemos configuar desde que tipo de generacion de mutantes vamos a utilizar, a si lo haremos concurrentemente o en un solo hilo, donde se generará el reporte, que clases testear o no, etc.

### Resultado de los Tests

Como resultado de las pruebas de mutación con Pitest en el reporte podemos tener los siguientes resultados:

1. Mutanta eliminado.
2. No ha sido cubierto por los test y por tanto el mutante ha sobrevivido. 
3. La mutación no es válida y no ha podido ser ejecutada por la JVM.
4. Un error en la memoria.
5. Un error de ejecución.

Una vez finalizado las pruebas tendrémos un report con el resultado de los test de mutación. También existe un plugin de SonarQube.

![pitest-report]({{ site.url }}{{ site.baseurl }}/assets/images/mutation-testing/pitest-report.png)

## [Stryker Mutator](https://stryker-mutator.io)

![pitest-report]({{ site.url }}{{ site.baseurl }}/assets/images/mutation-testing/stryker.png)

Esta herramienta para automatizar test de mutaciónes, está disponible actualmente para Js y Ts, C# y Scala. Entre sus características está la de soportar más de 30 tipos de mutaciones, ser Open Source, realizar los test de de una forma rápida y generar un reporte de los test que realiza.

En este [enlace](https://stryker-mutator.io/docs/mutation-testing-elements/supported-mutators/) podemos ver en qué consisten esos 30 tipos de mutaciones. Hay que ver también que dependiendo del lenguaje hay algunos tipos de operadores que no se soportan a la hora de generar mutantes.

[](https://www.notion.so/Mutation-Testing-ba27029528544907a5fa6ee7188c5a0d#1444f50f23db4f90bf5ecefd52a285a7)
