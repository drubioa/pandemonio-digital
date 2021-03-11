---
layout: post
title: "Sdkman"
date:   2021-03-11 19:00:00 +0100
categories: pildora
author: Diego Rubio Abujas
tags: java pildoras
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/sdkman/logo-sdkman.png)

En esta ocasión vengo a hablar de algo que llevaba tiempo queriendo montar en mi máquina y que me ha resultando de gran utilidad.

Pongámonos en situación amigos desarrolladores de **java**, cuántas veces nos ha pasado que tenemos un proyecto que debe ir con la JDK 1.8 y otro que queremos correr con la JDK 11, u 15. O digamos que tenemos un proyecto muy viejuno que va con la 1.6, muchas versiones de la maquina de java tediosas de instalar y de borrar. Configurar muchas variables de entorno en caso de windows.

Bien, [SdkMan](https://sdkman.io/) nos proporciona una aplicación que nos permite mediante una consola de comandos cambiar entre distintas versiones de Sdk, también nos permite tener distintas versiones de Maven, Groovy, Kotlin, Scala... etc.

## Instalación en Win10

La web nos proporciona una instalación muy sencilla para Linux y Mac, pero en nuestro caso vamos a instalarlo en nuestro Windows 10. La instalación en Windows 10 nos plantea un problema inicial, y es que se realiza con un script bash. Bien para la instalación lo que hemos hecho es utilizar la consola de GitBash que se nos instaló junto al [Git](https://gitforwindows.org/).

```bash
curl -s "https://get.sdkman.io" | bash
```

Una vez ejecutamos este comando en mi caso, me dio un error y es que no encuentra zip, unzip. Vaya, otro problema derivado de tener que instalarlo en Windows 10. Bien, la solución por lo que vi en StackOverflow es comentar el siguiente código.

```bash
#echo "Looking for unzip..."
#if ! command -v unzip > /dev/null; then
#	echo "Not found."
#	echo "======================================================================================================"
#	echo " Please install unzip on your system using your favourite package manager."
#	echo ""
#	echo " Restart after installing unzip."
#	echo "======================================================================================================"
#	echo ""
#	exit 1
#fi

#echo "Looking for zip..."
#if ! command -v zip > /dev/null; then
#	echo "Not found."
#	echo "======================================================================================================"
#	echo " Please install zip on your system using your favourite package manager."
#	echo ""
#	echo " Restart after installing zip."
#	echo "======================================================================================================"
#	echo ""
#	exit 1
#fi
```

Una vez comentado estos fragmentos de código ya podremos instalar el sdkman en nuestro windows 10, o al menos yo en mi caso pude.

## Utilización

Una vez instalado sdkman, desde la terminal de bash podemos realizar el siguiente comando: 

```bash
$ sdk list java
================================================================================
Available Java Versions
================================================================================
 Vendor        | Use | Version      | Dist    | Status     | Identifier
--------------------------------------------------------------------------------
 AdoptOpenJDK  |     | 15.0.2.j9    | adpt    |            | 15.0.2.j9-adpt
               |     | 15.0.2.hs    | adpt    |            | 15.0.2.hs-adpt
               |     | 11.0.10.j9   | adpt    |            | 11.0.10.j9-adpt
               |     | 11.0.10.hs   | adpt    | installed  | 11.0.10.hs-adpt
               |     | 11.0.9.open  | adpt    |            | 11.0.9.open-adpt
               |     | 8.0.282.j9   | adpt    |            | 8.0.282.j9-adpt
               |     | 8.0.282.hs   | adpt    |            | 8.0.282.hs-adpt
               |     | 8.0.275.open | adpt    |            | 8.0.275.open-adpt
 Alibaba       |     | 11.0.9.4     | albba   |            | 11.0.9.4-albba
 Amazon        |     | 15.0.2.7.1   | amzn    | installed  | 15.0.2.7.1-amzn
               |     | 11.0.10.9.1  | amzn    |            | 11.0.10.9.1-amzn
               |     | 8.282.08.1   | amzn    |            | 8.282.08.1-amzn
 Azul Zulu     |     | 15.0.2       | zulu    |            | 15.0.2-zulu
               |     | 15.0.2.fx    | zulu    |            | 15.0.2.fx-zulu
               |     | 11.0.10      | zulu    |            | 11.0.10-zulu
               |     | 11.0.10.fx   | zulu    |            | 11.0.10.fx-zulu
               |     | 8.0.282      | zulu    | installed  | 8.0.282-zulu
               |     | 8.0.282.fx   | zulu    |            | 8.0.282.fx-zulu
               |     | 6.0.119      | zulu    |            | 6.0.119-zulu
 BellSoft      |     | 15.0.2.fx    | librca  |            | 15.0.2.fx-librca
               |     | 15.0.2       | librca  |            | 15.0.2-librca
               |     | 11.0.10.fx   | librca  |            | 11.0.10.fx-librca
               |     | 11.0.10      | librca  |            | 11.0.10-librca
               |     | 8.0.282.fx   | librca  |            | 8.0.282.fx-librca
               |     | 8.0.282      | librca  |            | 8.0.282-librca
 GraalVM       |     | 21.0.0.2.r11 | grl     |            | 21.0.0.2.r11-grl
               |     | 21.0.0.2.r8  | grl     |            | 21.0.0.2.r8-grl
               |     | 20.3.1.2.r11 | grl     |            | 20.3.1.2.r11-grl
               |     | 20.3.1.2.r8  | grl     |            | 20.3.1.2.r8-grl
               |     | 19.3.5.r11   | grl     |            | 19.3.5.r11-grl
               |     | 19.3.5.r8    | grl     |            | 19.3.5.r8-grl
               |     | 19.1.0       | grl     |            | 19.1.0-grl
 Java.net      |     | 17.ea.12     | open    |            | 17.ea.12-open
               |     | 17.ea.11     | open    |            | 17.ea.11-open
               |     | 17.ea.10     | open    |            | 17.ea.10-open
               |     | 17.ea.9      | open    |            | 17.ea.9-open
               |     | 17.ea.2.pma  | open    |            | 17.ea.2.pma-open
               |     | 17.ea.2.lm   | open    |            | 17.ea.2.lm-open
               |     | 16.ea.36     | open    |            | 16.ea.36-open
               |     | 15.0.2       | open    |            | 15.0.2-open
               |     | 11.0.10      | open    |            | 11.0.10-open
               |     | 11.0.2       | open    |            | 11.0.2-open
               |     | 8.0.282      | open    | installed  | 8.0.282-open
               |     | 8.0.265      | open    | installed  | 8.0.265-open
 Mandrel       |     | 21.0.0.0     | mandrel |            | 21.0.0.0-mandrel
               |     | 20.3.1.2     | mandrel |            | 20.3.1.2-mandrel
 SAP           |     | 15.0.2       | sapmchn |            | 15.0.2-sapmchn
               |     | 11.0.10      | sapmchn |            | 11.0.10-sapmchn
 TravaOpenJDK  |     | 11.0.9       | trava   |            | 11.0.9-trava
================================================================================
Use the Identifier for installation:

    $ sdk install java 11.0.3.hs-adpt
================================================================================
```

Podemos visualizar un listado con las distintas versiones de java de los distintos proveedores, si deseamos instalar alguna de ellas únicamente tenemos que seleccionar install y se pondrá descargar e instalar esa sdk. Una vez finalizada la instalación tenemos la opción de ponerla como versión por defecto. 

Podemos desinstalar, cambiar version por defecto, utilizar una u otra versión. Y esto no solo se ciñe a java, también a maven, groovy, scala... etc.

Podemos observar la siguiente estructura de carpetas:

```bash
$ cd .sdkman/
$ ls
archives/  bin/  candidates/  contrib/  etc/  ext/  src/  tmp/  var/
$ cd candidates/
$ ls
java/  maven/  sbt/  scala/
$ cd java/
$ ls
11.0.10.hs-adpt/  15.0.2.7.1-amzn/  8.0.265-open/  8.0.282-open/  8.0.282-zulu/  current/
$ cd current/
$ ls
ADDITIONAL_LICENSE_INFO  ASSEMBLY_EXCEPTION  bin/  conf/  include/  jmods/  legal/  lib/  LICENSE  release  version.txt
```

Como podemos observar en candidatos tenemos instalados las distintas sdks, y de todas las que hay instalado en current tenemos la actual.

Faltaría, en nuestro caso que utilizamos windows 10, irnos a variables del entorno y configurar como

JAVA_HOME, MVN_HOME, SCALA_HOME, Path las rutas correspondientes a la ubicación donde hayamos instalado el sdkman. De esta forma cada vez que cambiemos la versión candidata se cambiará en nuestra terminal.

![]({{ site.url }}{{ site.baseurl }}/assets/images/sdkman/)


## [NVM](https://github.com/nvm-sh/nvm)

Al igual que sdkman, para desarrollos en node existe nvm (Node version manager) que al igual que sdk nos posibilita hacer switch entre distintas versiones de node.js y cuyo enlace dejo mas arriba.