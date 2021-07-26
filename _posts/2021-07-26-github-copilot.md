---
layout: post
title:  "GitHub Copilot"
date:   2021-07-25 06:33:00 +0100
categories: documentación
author: Diego Rubio Abujas
tags: opinión pildoras
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/github-copilot/logo.png)

En esta entrada vengo a hablar un poco sobre qué es **GitHub Copilot**, que nos puede ofrecer y qué posible futuro nos trae al mundo del desarrollo. El origen del interés de esta tecnología viene de un grupo que mantengo con antiguos excompañeros.

Según podemos observar en su [página web](https://copilot.github.com/) se trata de un **asistente al desarrollo**, el cual mediante inteligencia artificial y partiendo de comentarios genera la implementación de código. Hace algo parecido a *pair programming* pero con una IA.

- A partir de comentarios implementar funcionalidades como podemos ver en el siguiente ejemplo.

![]({{ site.url }}{{ site.baseurl }}/assets/images/github-copilot/snap1.png)

- Puede realizar una predicción del código que hay que repetir y autocompletarlo por nosotros.

![]({{ site.url }}{{ site.baseurl }}/assets/images/github-copilot/snap2.png)


- Definir tests a partir del nombre de lo que queremos testear. En el caso de que no se adecue del todo a lo que queremos también puede mostrar alternativas a las soluciones.

![]({{ site.url }}{{ site.baseurl }}/assets/images/github-copilot/snap3.png)


De hace tiempo atrás he observado como mi IDE Intellij cada vez me muestra sugerencias más acertadas, ya a día de hoy me tiene tan alado que prácticamente es escribir un carácter y ya prácticamente me sugiere la línea entera Esto es quizás un paso más, ya no solo es autocompletar la clase, método o nombre de variable; estamos hablando de que te implemente el método entero a partir de un comentario o un nombre de función. 

Por el momento Copilot no se encuentra disponible para todos los lenguajes y está en una fase **experimental**. Me he presentado a ver si me envían una invitación pero por lo que tengo entendido está como un plugin para **Visual Studio Code** y para lenguajes como *Python, Go, Javascript y Java*. 

En la siguiente imagen  podemos observar el funcionamiento de esta IA. 

![]({{ site.url }}{{ site.baseurl }}/assets/images/github-copilot/diagram.png)


Como podemos observar en la imagen este servicio utiliza para proveer la sugerencia **OpenAI Codex Model** del que vamos a hablar a continuación:

# OpenAI Codex Model

 Se trata de una **modelo de aprendizaje automático** desarrollado por **OpenAI** que impulsa GitHub Copilot. Los creadores de este modelo se inspiraron en *GPT-3* para su creación. Este modelo de aprendizaje automático se nutre de código publico existente en internet (Entiendo que en los repositorios públicos de GitHub entre otros).

De GPT-3 sabemos que se trata de un modelo de lenguaje, lo cual indica que su objetivo es realizar una predicción de lo que vendrá a continuación basándose en datos previos. Podría decirse que es como un "autocompletar" pero referente a codificación. Las respuestas dadas por este modelo de lenguaje son "posibilidades" pueden ser más o menos acertadas.

# Conclusiones

Hay que aclarar que esta IA **no sustituye al desarrollador**, la codificación es solo una parte de las muchas tareas que debe realizar un desarrollador. Los autores de OpenAI Codex Model señalan que su estado actual puede reducir costes de producción de software y aumentar la productividad pero no remplazará otras actividades de desarrollo.

Esta IA se nutre de código open souce subido a la plataforma de GitHub por otros desarrolladores. Por lo que he podido leer este nuevo sistema será **de pago** y **no estará bajo licencia copyleft**. La IA de Github utiliza codigo bajo licencia GPL para entrenarse y realizar su funcion de autocompletado: todo esto ha provocado cierto malestar y controversia entre la comunidad de desarrolladores ya que su código se está utilizando para que otra empresa pueda lucrarse con su trabajo aportado y hecho público.
