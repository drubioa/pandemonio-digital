---
layout: post
title: "GitHub Pages + Jekyll"
date:   2021-01-07 19:44:37 +0100
categories: demos
author: Diego Rubio Abujas
---

![Logo GitHub Pages]({{ site.url }}{{ site.baseurl }}/assets/images/github-pages-jekyll/jekyll_ghpages.jpg){:class="page.image"}

En esta entrada voy a dejar constancia de mis aventuras y desventuras tratando de crearme un blog en Git Hub Pages. ~~Aunque este blog ha sido creado con Blogger la idea inicial era crearlo en GitHub Pages, pero determinadas circunstancias, que indicaré mas abajo, me han hecho crearlo en esta plataforma~~.

Finalmente me he decido a llevar todo este blog a través de Git Hub Pages con Jekyll.

# ¿Qué es GitHub Pages?
Se trata de un servicio de alojamiento de páginas web estáticas proporcionado por GitHub. Este servicio está ideado para crear una web asociado a nuestro proyecto, o bien a nuestro usuario o a una organización relacionada con nuestro proyecto.

La dirección donde se crea nuestra web será `http(s)://<username>.github.io` seguido del nombre del proyecto. 
(Aunque creo podemos cambiar a otro dominios)

Este proyecto como requisito debe tratarse de una web estática. Para poder utilizarla dentro del portal de GitHub debemos irnos a las opciones del repositorio, hacer de éste un repositorio público y configurarlo como *GitHub pages*.

Para la construcción de la web estática se utiliza un generador de sitios estáticos. **GitHub Pages** nos sugiere **Jekyll**. 
Aunque hay otras soluciones, en un futuro post hablaré de **Hexo** también.

Una vez construido el sitio web se despliega en el GitHubPages. He observado que con Jekyll no es necesario definir Actions en el repositorio de GitHub. Es necesario incluir un fichero de configuración *config_.yml* en ambos casos, en el que se incluye configuración referente al repositorio.

# Requisitos de la prueba
La prueba que pretendemos realizar en esta entrada, es crearnos un blog (similar a este en el que escribo), mediante páginas web estáticas, y que este se construya y despliegue en GitHub Pages.

Además este blog tiene que cumplir los siguientes requisitos:

1. Es necesario que el blog permita la publicación de entradas.
2. Las entradas deberán estár categorizadas y etiquetadas.
3. La plantilla que se utilice deberá de adaptarse bien a dispositivos móviles, tabletas y ordenadores.
4. Deberá tener una interfaz amigable y moderna.
5. Estaría bien poder alternan entre *theme* dark y light.
6. Además estaría genial poder disponer de varios idiomas (ENG/SPA) en los post.
7. Una funcionalidad para que los usuarios puedan incluir comentarios.>
8. Un script para poder hacer analisis deade Google Analytics.

# ¿Qué es Jekyll?
> Jekyll es un generador simple para sitios web estáticos con capacidades de blog; adecuado para sitios web personales, de proyecto o de organizaciones. Fue escrito en lenguaje de programación Ruby por Tom Preston-Werner, el cofundador de GitHub, y se distribuye bajo la licencia de Código abierto MIT.
> 
>En lugar de usar bases de datos, Jekyll coge el contenido, en formato Markdown o Textile y plantillas Liqui4​ y produce como resultado un sitio web completo estático listo para ser presentado mediante un servidor web tal como Servidor HTTP Apache, Nginx u otro.5​ Jekyll es el motor de GitHub Pages,6​ una funcionalidad de GitHub que permite a los usuarios hospedar sitios web desde sus repositorios del propio GitHub.
>
> <cite>Wikipedia</cite>

Podéis encontrar mucha información sobre este genrador en su [portal](https://jekyllrb.com/).

De este generador me ha parecido muy interesante el tema de utilizar contenido en formato **Markdown** para generar los post del blog. Y bueno me ha llamado mucho la atención el hecho de que la web que se genere sea **contenido estático**, es decir que mediante una web hecha con código estático podemos hacer lo mismo un CMS como Wordpress, Drupal... etc, sin necesidad de una base de datos, ni se un backend. 