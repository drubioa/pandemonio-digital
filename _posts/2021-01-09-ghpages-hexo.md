---
layout: post
title:  "GitHub Pages + Hexo"
date:   2021-01-09 14:00:00 +0100
categories: demos
author: Diego Rubio Abujas
---

![Diagrama de arquitectura hexagonal]({{ site.url }}{{ site.baseurl }}/assets/images/demo-ghpages-hexo/hexo-github.png){:class="page.image"}

[Enlace web GitHub Pages](https://drubioa.github.io/demo-ghpages-hexo/)

[Repositorio GitHub](https://drubioa.github.io/demo-ghpages-hexo/)

# Introducción
Ya hemos realizado una prueba de concepto de la creación de un blog en GitHub Pages mediante Jekyll. En esta entrada vamos a mostrar cómo se genera un blog utilizando web estáticas y posteando con formato `markdown`, pero en lugar de utilizar el framework Jekyll vamos utilizar el framework `Hexo`.

# [Hexo](https://hexo.io)
Se trata de un framework para generar blogs mediante web estáticas utilizando **node**. Incluye funcionalidades que permiten publicar post, gestionar categorías, tags, temas, plugines etc. Es una solución bastante completa y muy similar a Jekyll. 

# Creando Blog con Hexo en GitHub Pages

## 1. Requisitos
Tener instalado [node](https://nodejs.org/es/) y `git`.

## 2. Generar blog con hexo en nuestro local
Para generar nuestro blog el primer paso será instalarnos el `hexo` con nuestro gestor de paquetes de node, y hacerlo `global` para poder utilizar el comando `hexo`. Una vez instalado el proceso de creación del blog es muy sencillo.

```bash
npm install hexo -g
hexo init [folder]
```
Esto nos creará el esqueleto de nuestro blog en la carpeta que hayamos indicado.

### 2.1 Creando nuevos post
Para crear un nuevo post unicamente tenemos que utilizar el siguiente comando

```bash
hexo new first-post
```
Esto nos generará un nuevo `post` en la carpeta `source\post`. Dentro tenemos nuestro fichero en formato `markdown` con una cabecera. Podemos incluir categorías, tags, etc.

```
---
title: first-post
date: 2021-01-10 18:19:29
tags:
---
First post
```

### 2.2 Generar site
Para generar el código html, css, js en nuestra carpeta `public` disponemos del comando  `generate`.

```bash
hexo generate
```

Tras lanzarlo podemos observar que se crea una nueva carpeta con el `index.html` y todo el contenido del blog.

### 2,3 Levantar nuestro blog en local

Al igual que con Jekyll, disponemos del comando `server` que levanta `public` en nuestro máquina local en el puerto que hayamos indicado (Por defecto `https://localhost:4000`).

```bash
hexo server
```
## 3. Aplicar un theme
Disponemos de un amplio surtido de themes disponibles en la siguiente [url](https://hexo.io/themes/). Para poder utilizarlos únicamente hay que seguir los pasos que nos indiquen en la documentación de su repositorio.

Para esta prueba que estamos realizando: utilizaremos el theme **cupertino**. Para aplicarlo seguimos las indicaciones del [repositorio](https://github.com/MrWillCom/hexo-theme-cupertino). 

Cambiamos el `_config.yml` y añadimos el theme, y clonamos el repositorio del theme en la carpeta `_themes`.

Una vez hayamos incluido el theme, si levantamos la aplicación podémos comprobar que ya aparece el nuevo theme y el topic que habíamos creado anteriormente.

## 4. Desplegando blog en GitHub Pages.
Ya tenemos nuestro blog levantado en local, con un post. Ahora vamos a desplegar este blog de prueba en GitHub Pages.

### 4.1 Creamos repositorio en GitHub
En primer lugar nos creamos un nuevo repositorio en GitHub. Este debe ser público. 

Una vez creado 

```bash
git init
git add .
git commit -m "First commit"
git remote add origin git@github.com:drubioa/demo-ghages-hexo.git
git push --set-upstream origin master 
```

Subimos los cambios que hemos realizado a `master`.

### 4.2 Configurando GitHub
Dentro de la web de GitHub debemos hacer dos cosas:

1. Crear una rama llamada `gh-pages` vacía por el momento, pero donde posteriormente irá el contenido de la carpeta `public` con nuestro blog.
2. Dentro de Ajustes habilitar la opción de GitHub Pages e indicar que utilice `root/` y que utilice la rama `gh-pages`. Cada vez que se despliegue algo sobre esta rama, se actualizará nuestra web con el contenido de esta rama.

Con la url que obtenemos nos iremos a nuestro fichero `_config.yml` y una vez aquí pondremos la ruta como indico más abajo para mi caso:

```yaml
# URL
## If your site is put in a subdirectory, set url as 'http://example.com/child' and root as '/child/'
url: https://drubioa.github.io
root: /demo-ghpages-hexo/
```
Es muy importante que root vaya entre '/'.

### 4.3 Desplegando nuestro blog en GitHub Pages

Para desplegar nuestro blog en primer lugar crearemos una nueva llamada `gh-pages` en el repositorio de GitHub. Seguidamente instalaremos en nuestro proyecto [hexo-deployer-git](https://www.npmjs.com/package/hexo-deployer-git) con el siguiente comando:

```bash
$npm install hexo-deployer-git --save
```

Una vez instalado debemos comprobar que en `package.json` dentro de script tengamos lo siguiente:

```json
  "scripts": {
    ...
    "build": "hexo generate"
    ...
```

Aparte en nuestro fichero `_config.yml` debemos incluir lo siguiente:

```yaml
deploy:
  type: git
  repo: YOUR_GITHUB_REPO.git
  branch: gh-pages
```
Una vez hayamos hecho todo esto cada vez que queramos desplegar nuestro blog en git hub pages únicamente tenemos que hacer lo siguientes:

```bash
$hexo deploy
```

Este comando generará `public` con nuestra web y lo subirá a la rama de `gh-pages`. Una vez subido GitHub Pages refrescará la web con los cambios.

# Resultado de la prueba

Tras realizar todo esto si entramos en la url de GitHub Pages podemos observar que se ha creado el blog con el tema quer hemos elegido y el post que hemos creado. Ya lo que habría que hacer es personalizar el tema, incluir los temas que te interesen, etc.

![Prueba]({{ site.url }}{{ site.baseurl }}/assets/images/demo-ghpages-hexo/demo-blog.gif){:class="page.image"}

# Migración
Hexo nos proporciona la opción de migrar nuestro post de otras soluciones como podrían ser Jekyll, WordPress, Jomla, etc a Hexo.
Para ello en su [web](https://hexo.io/docs/migration) nos indican los pasos a seguir.

