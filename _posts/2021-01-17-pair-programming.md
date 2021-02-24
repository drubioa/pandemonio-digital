---
layout: post
title:  "Pair Programming + Code with me"
date:   2021-01-17 03:00:00 +0100
categories: documentación
author: Diego Rubio Abujas
tags: pildoras
---

![pair-programming]({{ site.url }}{{ site.baseurl }}/assets/images/pair-programming/ikea.png)

En este post tengo la intención de hablar un poco del concepto *pair programming*, dar mi opinión personal y hablar de un *plugin* de *Intellij* que puede servir para hacer pair programming. Que conste, que nunca he trabajado pair programming solo lo conozco a nivel teórico.

# Pair programming
El *pair programming* o programación en pareja es una técnica de programación en la que en un mismo ordenador dos programadores desarrollan el trabajo. La idea es que uno de los programadores se encarga de teclear código y el otro piensa en la solución y en cómo debe programarse. 

La teoría dice que mediante esta técnica:
- Se codifica a un ritmo mas constante.
- Hay menos errores, 4 ojos ven mas que dos.
- El flujo de trabajo es continuo
- Se comparte mayor conocimiento.
- Se colectiviza la propiedad del código.
- Mejor moral y motivación.

Este método de trabajo se utiliza en muchos desarrollos ágiles, en concreto en la programación extrema (XP). 
Esta metodología de trabajo cuenta con dos roles:
- *piloto*: se encarga de escribir el código.
- *copiloto*: se encarga de supervisar el código.

## Mob programming
El mob programming es aumentar el número de *copilotos* haciendo que un grupo de 2 o más personas rodeen al desarrollador. 

# Code with me
![code-with-me-logo]({{ site.url }}{{ site.baseurl }}/assets/images/pair-programming/code-with-me.svg)
[Enlace del plugin](https://plugins.jetbrains.com/plugin/14896-code-with-me)

Se trata de un servicio creado por JetBrains para el IDE Intellij y que posibilita el pair programming remoto. Es decir que dos desarrolladores que estén tele-trabajando y tengan el Intellij puedan conectarse ver lo que están haciendo en tiempo real y tocar el mismo código del proyecto de manera simultánea. 

En esta [web](https://www.jetbrains.com/help/idea/code-with-me.html#cwm_settings) podéis encontrar las opciones y funcionalidades que nos ofrece este curioso plugin. La idea es resumidamente que un IDE Intellij establezca una llamada otro,y ademas de poder compartir voz, tener un chat, se pueda tocar simultáneamente el mismo código.

# Opinión
![dilvert]({{ site.url }}{{ site.baseurl }}/assets/images/pair-programming/good-coders.gif)

En mi humilde opinión, el *pair programming* está muy bien en determinadas circunstancias en las que el equipo de desarrolladores se enfrenta a problemas complejos y dos o más miembros del equipo se sientan juntos para solucionarlo. Ahora se ve mucho con la situación actual y el hecho de estar todos en remoto la situación de algún miembro del equipo compartiendo pantalla y el resto viendo lo que hace, y comentando o sugiriendo soluciones. 

Veo esta forma de trabajar útil en determinadas circunstancias, pero en su justa medida. Es decir, trabajar de facto todos los días y toda la jornada en pair programming no me parece bien. Hay tareas, que hace un programador, que puede hacer perfectamente solo sin un compañero al lado. La situación de una labor rutinaria, sencilla implica que el copiloto se aburra y acabe o bien entreteniendo al piloto con conversación, o bien provoque que acabe aburriéndose y desmotivándose.

En resumen, pair programming para determinadas situaciones, para el día a día veo mejor trabajar solos.