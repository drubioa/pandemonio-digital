---
layout: post
title:  "Checkstyle maven plugin"
date:   2021-05-17 12:00:00 +0100
categories: testing
author: Diego Rubio Abujas
tags: testing
---

![Logo]({{ site.url }}{{ site.baseurl }}/assets/images/checkstyle/checkstyle.png){:class="page.image"}


Una de las cosas que me resultaron muy interesante tras mi breve paso por el mundo de Angular y Typescript era la posibilidad de incluir en la construcción del proyecto unos chequeos de estilo, de forma que podemos incluir reglas de codificación que deben cumplirse por todo el equipo de desarrollo. Con normas de estilo me refiero a que , por ejemplo, se deje un espacio en blanco, los nombres de la variables no empiecen por miyúusculas, o cualquier cosilla que se nos ocurra, por ejemplo que determinadas variables empiecen por "_" o cosas así. De esta forma tras construir nuestra aplicación no solo controlamos testeo, que sea compilable y que cumpla con sonar, sino que podemos definir nuestras propias normas de calidad de código.

Bien, pues para maven existe un plugin que nos permite definir nuestras propias reglas de estilo, y que como parte del proceso de construcción introduce una nueva fase llamada `checkstyle` en la que se comprueba que todo el código sigue unas reglas definidas por defecto o bien en un fichero xml. 

Además es posible incluir en nuestro pipeline de construcción una comprobación para que como parte del proceso de construcción, testeo y despliegue se compruebe que el código sigue las reglas de estilo que queramos definir.

# Links de interés:

[Apache Maven Checkstyle Plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin)

[Web de Checkstyle](https://cckstyle.org)

[Ejemplo de reporte de Checkstyle](http://maven.apache.org/plugins/maven-checkstyle-plugin/checkstyle.html)

[Ejemplo de Baeldung](https://www.baeldung.com/checkstyle-java)

# Ventajas

- Facilita le mantenimiento de código
- Asegura la calidad
- Facilidad de uso
- Permite implementar nuestras propias reglas de estilo

# Estándares de codificación en Java

Aunque podemos definir nuestros propios estándares de codificación. Para desarrollo en Java hay dos convenciones muy utilizadas, una la sugerida por Sun y otra por Google. Incluyo más abajo links a las web donde se especifican todas las normas de codificaciones de cada uno de esto estándadres y que pueden ser configurados en el plugin.

## [Java Code Convention](https://www.oracle.com/technetwork/java/codeconventions-150003.pdf)

Esta convención fue desarrollada por Sun. Destaca la importancia de tener una codificación con convenciones similares basándose en que:

- El 80% del tiempo lo pasamos haciendo correcciones y mantenimiento.
- Raramente el mantenimiento es realizado por el propio autor.
- Seguir una convención de codificiación hace el código más legible.
- Necesidad de limpieza en el código que se entrega.

### [Resumen de las conventions que se sugieren](https://gist.github.com/goldbattle/9283399)

## [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)

Otra interesante convención desarrollada por Google para Java.