---
layout: post
title:  "Configurando domain en GitHub Pages"
date:   2021-01-12 01:30:00 +0100
categories: configuración
author: Diego Rubio Abujas
---

![Logo GhPages]({{ site.url }}{{ site.baseurl }}/assets/images/ghpages-config-domain/ghpages.jpeg){:class="page.image"}

Una vez hemos creado nuestra web en *GitHub Pages* (como indicamos anteriormente) dispondremos de una url desde la cual podremos acceder a ella `http(s)://<username>.github.io/<repository>`. Es posible que a muchos les sirva esta url, pero también es muy posible que queráis disponer de vuestro propio *domain* (.com, .es, .tk...). En esta pequeña *pildorilla* de post, indicaré los pasos que hay que seguir para configurar un domain en GitHub Pages. 

Partiremos de que ya habéis adquirido el dominio.

# Paso 1: Configurar DNS en nuestro proveedor de dominio
Debemos acceder a nuestro proveedor del Dominio y realizar unos cambios en las DNS.

1) Eliminar todas las reglas de tipo **A** (Direcciçon IPv4) y **AAA** (IPv6) de subdominios y en su lugar dejas las siguientes:

| TIPO | NOMBRE DE HOST |
| --- | --- | 
| A	| @ | 185.199.108.153 |	
| A	| @	| 185.199.109.153 |	
| A	| @	| 185.199.110.153 |	
| A	| @	| 185.199.111.153 |

Es muy importante que NO añadáis ningún registro DNS para `www` en las cuatro reglas de más arriba.

2) Crear un alias para otro dominio o sitio web Solo para subdominios. Aquí configuraremos nuestro dominio de GitHab Pages.

| TIPO | Nombre del Host | Apunta a |
| --- | --- | --- | 
| CNAME | www | <usuario>.github.io |

# Paso 2: Configurar dominio en GitHub Pages
El primer paso es entrar desde la web de GitHub al apartado de configuración. Aquí tenemos que hacer dos cosas:

![Logo GhPages]({{ site.url }}{{ site.baseurl }}/assets/images/ghpages-config-domain/screenshot.png){:class="page.image"}

1. En primar lugar en Custom domain introducir nuestro dominio, sin http/https, solo el nombre `.com,.es,.net...`. 
Al habilitar esta opción se nos crearé de manera automática un fichero `CNAME` con la dirección de nuestro dominio en el directorio raíz del repositorio.

2. Habilitar HTTPS, esta operación puede tardar bastante en aparecer (Hasta 24 hora de hecho).

# Paso 3: Configurar nuevo dominio en Jekyll
Debemos modificar nuestro fichero `_config.yml` para modificar el baseurl y url. Quedaría de la siguiente manera para, por ejemplo este blog.

```yaml
baseurl: "" 
url: "https://pandemoniodigital.es" 
```
Finalizada la configuración y una vez se habilita la opcion `https` en la configuración GitHub ya podremos acceder desde el dominio a nuestra web creada con Jekyll/Hexo.
   

