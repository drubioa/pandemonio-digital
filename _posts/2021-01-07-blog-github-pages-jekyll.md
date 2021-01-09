---
layout: post
title: "GitHub Pages + Jekyll"
date:   2021-01-07 19:44:37 +0100
categories: demos
author: Diego Rubio Abujas
---

![Logo GitHub Pages]({{ site.url }}{{ site.baseurl }}/assets/images/github-pages-jekyll/jekyll_ghpages.jpg){:class="page.image"}

[Enlace web GitHub Pages](https://drubioa.github.io/demo-ghpages-jekyll/)

[Repositorio GitHub](https://github.com/drubioa/demo-ghpages-jekyll)

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

## Ventajas de Jekyll

Recientemente leí una entrada de un blog llamada [Why did I choose Jekyll over WordPress?!](https://blog.webjeda.com/why-jekyll-over-wordpress/) en el cual mediante una comparativa entre el CMS Wodpress y Jekyll destacan algunas de las virtudes y ventajas de utilizar una web estática generada con Jekyll frente a Wordpress para crear un blog. Básicamente destacar lo siguientes:

1. Wordpress (y otros CMS ) tienen que tener todo el core para gestionar el blog, plugins, librerías una base de datos, etc. Todo esto las hace muchísimo más pesadas que el `_site` generado por Jekyll que es un conjunto de recursos, html, css y js generador para desplegarse directamente en GitHub Pages, Apache , Nginx o el servidor que sea. 

2. Jekyll no tiene parte de back, no tiene que estar realizando consultas de base datos, llamando a otros servicios, etc. Únicamente tiene que renderizar la web estática y cargar sus scripts de javascript. Con todo esto conseguimos una velocidad mayor que la que tendríamos con Wordpress u otros CMSs.

3. Escribir un post en un blog como Wordpress ó Blogger requiere de que este sea escrito en `html` mientras que con Jekyll tenemos el formáto `markdown`que nos permite redactar las entradas de una forma más rápida y elegante.

## Desventajas de Jekyll
No es oro todo lo que reluce, al generar mi blog con Jekyll me he encontrado con muchas trabas y cosillas con las que me he tenido que pelear. A continuación enumero algunas de ellas.

1. Generar un blog con Jekyll no es apto para todo el mundo, no es tan intuitivo como podría ser Wordpress o Blogger. Requiere picar, requiere leerse documentacion, tocar configuraciones, instalarte Ruby y ejecutar comandos de consola. Tienes que gestionar con git un repositorio con GitHub y configurarlo para que te levante tu web en GitHub Pages. 

2. Cuidado con las versiones, plugines y themes que utilizas. Uno de los problemas que tuve en mi primer intento de crearme el blog es que en mi local funcionaba perfectamente pero en GitHub Pages no, y es que ha que tener en cuenta que no todas las versiones de Jekyll y algunas dependencias son soportadas por GitHub Pages. Dejo un [enlace](https://pages.github.com/versions/) en el que se pueden consultar qué versiones de Jekyll y sus librerías son soportadas.

3. La búsqueda de un theme adecuada. Di muchas vueltas hasta dar con un theme que me gustase, no se si es que yo soy muy selectivo o que el abanico de themes de Jekyll es muy limitado. Hay que tener en cuenta que este es el tercer theme que he probado, los dos anteriores me dieron problemas al subirlo a GitHub Pages. También hay que decirlo yo no soy diseñador ni maquetador, me cuesta horrores combinar colores, y pegarme con los estilos. Lo suyo sería partir del theme `minima` (el que trae por defecto jekyll) y ponerlo a tu gusto, pero yo elegí coger un theme ya creado y retocarlo un poco.

# Creando un blog en GitHub Pages con Jekyll
## Paso 1: Creando repo en GitHub
Seguimos los pasos que nos indica GitHub en su [documentación](https://docs.github.com/es/free-pro-team@latest/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll).

Partimos de que ya tenemos una cuenta en GitHub, para este proyecto debemos crear un repositorio público (Debe ser público para poder public la web en GitHub Pages de manera gratuita). Lo creamos vacío y pasamos a trabajar desde nuestro local

## Paso 2: Crear el site con Jekyll en local
Siguiendo los pasos que nos indica [Jekyll](https://jekyllrb.com/docs/).

Debemos de cumplir los requisitos en función del Sistema Operativo. En mi caso, Windows 10, tuve que instalarme Ruby y Jekyll. Ruby lo instalé con un instalador, y Jekyll por linea de comandos.

## Paso 3: Levantando nuestro site en local
Una vez hemos creado nuestro proyecto jekyll hay tres comandos que utilizaremos bastante. Así los lanzo en Windows 10 (Imagino que en Linux/Mac irán de otra manera).

Este comando nos permite construir `_site` con todo el contenido estático generado que se desplegará en la web.
```bash
$bundle exec jekyll build 
```

El siguiente comando lanza en `localhost:4000` la web con el contenido de `_site`
```bash
$bundle exec jekyll server 
```

Y finalmente este comando elimina el contenido de `_site`
```bash
$bundle exec jekyll clean 
```

## Paso 4: Eligiendo un theme
Cuando lancemos la primera vez veremos nuestro blog hecho con Jekyll con el theme `minima`. Existe varios portales de themes como podemos ver en la [web de Jekyll](https://jekyllrb.com/resources/). Pero ojo, que no todos los themes son soportados por GitHub Pages, en este [enlace](https://pages.github.com/themes/) se muestra nlos themes que indica GitHub que son soportados. Aunque a decir verdad hay otros que son soportados.

Yo tras mucho recorrerme themes opté por uno remoto, del repositorio [Basically Basic Jekyll Theme](https://github.com/mmistakes/jekyll-theme-basically-basic). Pero como digo es cuestión de ir probando vários y ver cuál os gusta mas y despliega bien en GitHub Pages.

## Paso 5: Configurando el blog y publicando post
Para configurar jekyll para poder desplegarlo en GitHub Pages he tenido que realizar varios cambios en el fichero `_config.yml`. 

En primer lugar, es necesario incluir la url que nos generará GitHub Pages cuando lancemos la app
```yaml
baseurl: "/<repository>" # the subpath of your site, e.g. /blog
url: "https://<usuario-github>.github.io" # the base hostname & protocol for your site, e.g. http://example.com
```

También he cambiado en el ``Gemfile` que en lugar de Jekyll utilice la dependencia de `github-pages`.

```
gem "github-pages", group: :jekyll_plugins
```

### ¡Ojo! ruta de las imágenes y recursos
Dado que vamos a utilizar una baseurl cuando queramos incluir recursos, como por ejemplo el enlace a una imagen, el favicon u alguna descarga. Deberemos incluir esa ruta en la ruta al recursos. A continuación muestro un ejemplo de cómo estoy incluyendo imágenes en mis posts para que veáis como tengo que incluir los recursos.

```
![Alt de la imagen]({{ site.url }}{{ site.baseurl }}/assets/images/image.png){:class="style"}
```

## Paso 6: Configuración de GitHub Pages
Una ves tenemos subido nuestros cambios al repositorio de GitHub tenemos que habilitar la opcion de GitHub Pages. Para ello vamos a `Settings`y una vez alli habilitamos la opción de GitHub Pages. Podemos si lo deseamos y disponemos de uno cambiar el dominio.

![Logo GitHub Pages]({{ site.url }}{{ site.baseurl }}/assets/images/github-pages-jekyll/github-pages-screnshot.png){:class="page.image"}

Tras unos segundos, nos aparece un link en verde pulsamos sobre él y *voila* se nos abrirá la web a nuestro blog.

## Creando post
Los post tienen un formato muy simple se crean en la carpeta `_post` con el formato `yyy-mm-dd-nombre-del-post.md`. Estos post tienen en su cabecera información sobre la categoria, tagas, autor, fecha de creación etec.

Cada vez que hagamos `git push`a la rama que hayamos indicado (en mi caso `master`).

# Paso 7: Comentario
Una de las dudas que me surgió al principio fue, y si el sitio web es estático ¿Cómo se gestionan los comentarios de las entradas?.No hay base de datos, ni persistencia. La solución que nos plantea Jekyll es incluir disqus.

## ¿Qué es Disqus?
>Disqus (/dɪsˈkʌs/) is an American blog comment hosting service for web sites and online communities that use a networked platform. The company's platform includes various features, such as social integration, social networking, user profiles, spam and moderation tools, analytics, email notifications, and mobile commenting. It was founded in 2007 by Daniel Ha and Jason Yan as a Y Combinator startup.
>
> -- Wikipedia

Dandonos de alta en su [web](https://disqus.com) y asociando nuestra url podemos disponer de un servicio que se encargará de almacenar los comentarios de los post de nuestra web. Ya será labor nuestra moderar y administrar de allí.

# Paso 8: Google Analytics
La mayoría de los themes incluyen la opción de en el `_config.yml` incluir el id para generar en el `head` un script que permita realizar un seguimiento por parte de [Google Analytics](https://analytics.google.com/), ya es coger la documentación de cada topic.


