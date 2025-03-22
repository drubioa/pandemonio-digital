---
layout: post
title:  "Patrón A-A-A Vs enfoque GWT"
date:   2025-03-21 22:00:00 +0100
categories: arquitectura
author: Diego Rubio Abujas
tags: testing 
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/aaa-vs-gwt/software-testing.jpg){:class="page.image"}

# Patrón A-A-A

El patrón AAA es una metodología utilizada para elaborar pruebas unitarias. Se utiliza ampliamente en Java, C++, JS, Python y otros lenguajes estructurados y orientados a objetos en el que se emplean pruebas unitarias

Basicamente consiste en estructurar el test en tres bloques:

## Arrange: configurar los datos

En este bloque se incluyen todas las condiciones del test, se instancia los objetos, se mockean los objetos y sus respuesta. Se inicializan las variables. 

En este bloque correspondería en caso de estar con JUNIT y Mockito ir indicando que respuesta y comportamiento esperaremos de cada uno de los mocks.

## Act: ejecutar el método a probar

En esta fase, se llama a la función o método que se va a probar, usando los objetos o valores preparados en la fase anterior. 

## Assert: comprobar que el resultado es el esperado

En esta fase, se comprueba si el resultado de la ejecución es el esperado. Aquí se verifican las condiciones, como si el valor devuelto es el correcto o si el estado de los objetos ha cambiado de acuerdo a lo esperado.

También se verifican si se han hecho las invocaciones que se esperan y se han invocado los objetos mockeados que esperabamos.

# Given When Then

## Given: Dado

Define el estado inicial o contexto en el que se va a ejecutar la prueba. Es similar a la fase "Arrange", donde se configuran las condiciones necesarias para que la prueba funcione.

## When: Cuando

Similar al Act de A-A-A.

## Then: Entonces

Se define el resultado esperado de la acción tomada, similar a la fase "Assert" del patrón A-A-A. Describe lo que debería ocurrir como resultado de la acción.

# Diferencias principales

¡Son practicamente iguales! Ambos patrones cumplen la misma función de estructurar pruebas, pero la elección entre uno u otro depende del contexto y las preferencias del equipo de desarrolladores. Hay que tener en cuenta que el patrón Given When Then va mas enfocado a describir comportamientos mientras que A-A-A está mas orientado a la parte técnica.

Incluimos una traducción de una entrada de [https://softwareengineering.stackexchange.com](https://softwareengineering.stackexchange.com/) 

> 
> 
> 
> AAA is very useful for me when I'm testing my own code. If I'm working on a project or a library for myself, AAA is the way that I go. It lets me set up whatever I need to execute my test and then *just test it*. It's quick to setup, and quick to verify that my code is working as I expect.
> 
> GWT is useful in business environments, where work that is done by programmers needs to be mapped to business value. Business value is mapped by features, and hopefully features that don't introduce bugs. There are many strategies for mapping features to programming tasks, but one of them is through requirements. In my experience, requirements range from user-level requirements all the way down to small tasks for the user to execute. This is useful because **it's easy for the managers to understand how the work the programmer is doing is impacting their customers/users, and therefore why the programmers are adding value to their business**
> 

En mi opinión, NO hay diferencias son exactamente lo mismo y no veo mucho sentido al debate. Eso si, hay que acordar con todo el equipo de desarrolladores si seguir una u otra nomenclatura a la hora de comentar el código.