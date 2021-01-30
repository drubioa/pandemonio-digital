---
layout: post
title: "Mi top 5 de plugins de Intellij"
date: 2021-01-08 14:00:00 +0100
categories: ides
author: Diego Rubio Abujas
tags: ide
---

![logo intellij]({{ site.url }}{{ site.baseurl }}/assets/images/top-plugins-intellij/logo-intellij.png){:class="page.image"}

En esta entrada voy a enumerar los 5 plugins más útiles e interesantes que he encontrado en estos años que llevo utilizando el IDE [Intellij](https://www.jetbrains.com/es-es/idea/). Quiero aclarar, que estos plugines me han ayudado a mi en mi desarrollo como backend trabajando con Java, Spring Boot, Groovy y algo de kotlin. Entiendo que hay plugines muy enfocados a otras librerías, lenguajes de programación con las que por el momento no me he tenido que pelear.

# 1. [Lombok](https://plugins.jetbrains.com/plugin/6317-lombok)

![logo lombok]({{ site.url }}{{ site.baseurl }}/assets/images/top-plugins-intellij/lombok-logo.svg){:class="page.image.caption"} 
En primer lugar mencionamos este indispensable plugin que lo que hace es permitirnos habilitar las anotaciones de lombok en nuestros proyectos. 

Lombok, para aquellos que no lo conozcan dejo un resumen de lo que es lombok:

>Project Lombok is a java library that automatically plugs into your editor and build tools, spicing up your java.
Never write another getter or equals method again, with one annotation your class has a fully featured builder, Automate your logging variables, and much more.
>
> -- [Project Lombok]( https://projectlombok.org)

# 2. [Sonar Lint](https://plugins.jetbrains.com/plugin/7973-sonarlint)

![logo sonarlint]({{ site.url }}{{ site.baseurl }}/assets/images/top-plugins-intellij/sonar-lint.png){:class="page.image.caption"} 
Este plugin fue todo un descubrimiento, muy útil para aplicar buenas prácticas de calidad en nuestro desarrollo. Nos permite analizar nuestro código cómo lo haría Sonar, pero sin tener que configurar Sonar, subir nuestro código y demás. Cada vez que detecta algo que puede provocar un aviso de bug en Sonar te subraya el código de color azul y te muestra por qué Sonar te llamará la atención y posibles soluciones. No solo es aplicable a proyectos de Java, tambien es posible configurarlo para otros lenguajes de programación. Además también tiene su [plugin para visualStudioCode](https://www.sonarlint.org/vscode).

Os dejo una definición de la web del plugin resumiendo lo que hace:

> SonarLint is a free IDE extension that lets you fix bugs and vulnerabilities as you write code! Like a spell checker, SonarLint highlights coding issues on the fly, with clear remediation guidance so you can fix them before the code is even committed. Across popular IDEs (Eclipse, IntelliJ, Visual Studio, VS Code) and popular programming languages, SonarLint helps all developers write better and safer code!
>
>SonarLint supports all JetBrains IDE, including IntelliJ, WebStorm, PhpStorm, PyCharm and RubyMine. It can analyze code written in Java, JavaScript, TypeScript, Python, Kotlin, Ruby, HTML & PHP.
>
>If your project is analyzed on SonarQube or on SonarCloud, SonarLint can connect to the server to retrieve the appropriate quality profiles and settings for that project. Java 8 is required to run SonarLint; analysis of JavaScript and TypeScript requires Node.js >= 8.
> 
> -- web del pluguin

# 3. [Key Promoter X](https://plugins.jetbrains.com/plugin/9792-key-promoter-x)

![logo keypromoter]({{ site.url }}{{ site.baseurl }}/assets/images/top-plugins-intellij/keypromoter.png){:class="page.image.caption"} 
Para los amantes de los atajos te teclados y ya que Intellij tiene atajos de teclado para casi todo. Este plugin lo descubrí en un taller de optimizar tu desarrollo con Intellij. Lo que hace es básicamente cada vez que hagamos algo utilizando el ratón, nos muestra un mensaje recordándonos que atajos de teclado existen para eso que hemos hecho. Además lleva un conteo de las veces que hacemos una funcionalidad sin utilizar atajos de teclado. 

Si vemos que nunca vamos a utilizar ese atajos de teclado, disponemos de la opción de ignorarlo para que no nos lo muestre en el futuro.

He de reconocer que hay algunos atajos de teclado que no son demasiado intuitivos en Intellij, sobretodo los de construir con maven, y debuguear, pero otros si que me se me han quedado grabados a fuego gracias a este interesante plugin.

# 4 [Rainbow Brackets](https://plugins.jetbrains.com/plugin/10080-rainbow-brackets)

![logo rainbow]({{ site.url }}{{ site.baseurl }}/assets/images/top-plugins-intellij/rainbow.png){:class="page.image.caption"} 
Este curioso pluging es muy util cuando nos encontramos con infinidad de llaves que se abren t se cierra. Cosa que veo mucho últimamente con los lamdas y la programación funcional. La idea es que caca llave, parentesis que abre le asigna un color, asi cuando se cierran se ve a qué paréntesis,llave,corchete corresponde.

# 5 [Codota](https://plugins.jetbrains.com/plugin/7638-codota-ai-autocomplete-for-java-and-javascript)

![logo codota]({{ site.url }}{{ site.baseurl }}/assets/images/top-plugins-intellij/codota.svg){:class="page.image.caption"} 
Este plugin también fue un descubrimiento interesante. Recurre a repositorios de código para que cuando estemos desarrollando mostrarnos ejemplos reales de otros proyectos de las clases, metodo que estemos utilizando. Así nos podemos bajar en otros proyectos existentes para ver cómo implementar librerias o funciones.
