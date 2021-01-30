---
layout: post
title:  "Demo Cucumber + Spring Boot"
date:   2021-01-11 03:11:00 +0100
categories: testing
author: Diego Rubio Abujas
tags: springboot testing
---

![Logo cucumber]({{ site.url }}{{ site.baseurl }}/assets/images/cucumber-spring-boot/cucumber.png)

[Prueba de Concepto](https://github.com/drubioa/demo-cucumber-spring-boot)

En este post vamos a hablar un poco de la pelea de esta semana: montar un proyecto con spring-boot y realizar pruebas siguiendo una estrategIa BDD (Behavior Driven Development). También especificaremos un poco qué es TDD, qué es Cucumber, Gherkins y cómo se implementan test de este tipo en un proyecto de Spring Boot.

# Un poco de teoría
## TDD 
>Desarrollo guiado por pruebas de software, o Test-driven development (TDD) es una práctica de ingeniería de software que involucra otras dos prácticas: Escribir las pruebas primero (Test First Development) y Refactorización (Refactoring). Para escribir las pruebas generalmente se utilizan las pruebas unitarias (unit test en inglés). En primer lugar, se escribe una prueba y se verifica que la nueva prueba falla. A continuación, se implementa el código que hace que la prueba pase satisfactoriamente y seguidamente se refactoriza el código escrito. El propósito del desarrollo guiado por pruebas es lograr un código limpio que funcione. La idea es que los requisitos sean traducidos a pruebas, de este modo, cuando las pruebas pasen se garantizará que el software cumple con los requisitos que se han establecido.
>
> --Wikipedia

Hay varias formas de llevar a cabo esta práctica, nosotros vamos a visualizar a continuación una propuesta por Nat Pryce y Steve Freeman en su libro [Growing Object-Oriented Software Guided by Tests](http://www.growing-object-oriented-software.com/) y que consiste en realizar estos tres pasos de forma cíclica hasta implementar todas las funcionalidades:

![Diagrama TDD]({{ site.url }}{{ site.baseurl }}/assets/images/cucumber-spring-boot/tdd.png)

1. Escribir un test para una funcionalidad, partiendo de la funcionalidad más básica, y que falle al no estar aún implementada la lógica de la funcionalidad.
2. Implementar la funcionalidad y hacer que el test pase satisfactoriamente.
3. Refactorizar el código, esta no debería afe tar a la funcionalidad del código.
   
## BDD
>BDD es un proceso de desarrollo de software que trata de combinar los aspectos puramente técnicos y los de negocio, de manera que tengamos un marco de trabajo, y un marco de pruebas, en el que los requisitos de negocio formen parte del proceso de desarrollo.

BDD (behavior-driven development (BDD) o desarrollo guiado por el comportamiento) se trata de un proceso de testeo de software en el cual nos centramos en el comportamiento que espera que tenga nuestra aplicación el usuario. A diferencia de TDD aquí esperamos que sea el usuario, o propietario de la aplicación quien nos deberá definir cómo quiere que se comparte la aplicación en lugar de nosotros como desarrolladores. Y os preguntaréis cómo va a definir el usuario o propietario el comportamiento de la aplicación si en la mayoría de las ocasiones este no posee conocimientos técnicos ni de programación, bien ahí es donde entra la existencia de un idioma común entre la parte de negocio y la parte técnica para definir el comportamiento de la funcionalidad que vamos a desarrollar.

Es importante a la hora de definir BDD comprender bien la fórmula "Given-When-Then" a la hora de definir pruebas de comportamiento.

- **Given (Dado que):** Aquí se específica el escenario inicial antes de la realización de la prueba. Por ejemplo supongamos que un usuario no existe y queremos darlo de alta, especificamos los datos del usuario y dejamos claro que no existe previamente.
- **When (Cuando):** Aquí se especifica la condición de la prueba. Se invoca la creación de un nuevo usuario.
- **Then (Entonces):** Postcondición, y comprobaciones oportunas. Se comprueba y demuestra que el usuario se ha creado satisfactoriamente.

## Gherkins
>Gherkin is the language that Cucumber uses to define test cases. It is designed to be non-technical and human readable, and collectively describes use cases relating to a software system. The purpose behind Gherkin's syntax is to promote behavior-driven development practices across an entire development team, including business analysts and managers. It seeks to enforce firm, unambiguous requirements starting in the initial phases of requirements definition by business management and in other stages of the development lifecycle.
>
>--Wikipedia

Podemos ver un ejemplo a continuación:

```gherkin
Scenario Outline: A user withdraws money from an ATM
    Given <Name> has a valid Credit or Debit card
    And their account balance is <OriginalBalance>
    When they insert their card
    And withdraw <WithdrawalAmount>
    Then the ATM should return <WithdrawalAmount>
    And their account balance is <NewBalance>

    Examples:
      | Name   | OriginalBalance | WithdrawalAmount | NewBalance |
      | Eric   | 100             | 45               | 55         |
      | Gaurav | 100             | 40               | 60         |
      | Ed     | 1000            | 200              | 800        |
```
En el portal de cucumber podemos encontrar una [documentación](https://cucumber.io/docs/gherkin/reference/) completa sobre la sintaxis de gherkin.

## Cucumber
>Cucumber is a software tool that supports behavior-driven development (BDD). Central to the Cucumber BDD approach is its ordinary language parser called Gherkin. It allows expected software behaviors to be specified in a logical language that customers can understand. As such, Cucumber allows the execution of feature documentation written in business-facing text. It is often used for testing other software. It runs automated acceptance tests written in a behavior-driven development (BDD) style.
>
>Cucumber was originally written in the Ruby programming language and was originally used exclusively for Ruby testing as a complement to the RSpec BDD framework. Cucumber now supports a variety of different programming languages through various implementations, including Java and JavaScript. The open source port of Cucumber in .Net is called SpecFlow. For example, Cuke4php and Cuke4Lua are software bridges that enable testing of PHP and Lua projects, respectively. Other implementations may simply leverage the Gherkin parser while implementing the rest of the testing framework in the target language.
>
>--Wikipedia

Podemos encontrar mucha información sobre este software en su [web](https://cucumber.io/). Nosotros para nuestro caso de prueba nos importaremos tres paquetes de cucumber que ya indicaré más en adelante cuando comencemos a implementar la prueba.

# La Prueba de concepto
Para este caso la prueba que vamos a realizas es la siguiente: vamos a desarrollar con spring-boot un servicio REST que reciba dos valores y devuelva la suma de ambos, algo simplón pero para el caso nos sirve. Lo que predentemos con esta prueba mas que desarrollar un servicio es incluir pruebas de comportamiento con cucumber y gherkins.

## Creación de la aplicación base
Vamos a [Sprint Initializr](https://start.spring.io/) y generamos un proyecto Spring Boot tal y como mostramos en la ventana de más abajo:

![Captura]({{ site.url }}{{ site.baseurl }}/assets/images/cucumber-spring-boot/spring-initializr.png)

## Implementando nuestro proyecto
No voy a indicar todos los pasos que he seguido para implementar la prueba de concepto, para saber que se ha hecho podéis echar un vistado al repositorio de GitHub (más arriba está el enlace), lo que sí quería indicasr es un plugin muy útil para Intellij que nos ayudará a trabajar de una manera más cómoda con el fichero de *features* del jherkins y que tieneis disponible en el siguiente [enlace](https://plugins.jetbrains.com/plugin/9164-gherkin) o bien entrando desde el marketplace de plugins de Intellij.

### API Rest
El proyecto no tiene demasiado, se trada de una aplicacion spring boot con los siguientes servicios REST:

| url | type | description |
| --- | --- | --- |
| examples/hello?name={nombre} | GET | Devuelve la cadena 'Hello ' + {nombre} |
| examples/sum?a={a}&b={b} | GET | Devuelve la suma de {a} y {b} |

## Cucumber


### Jerkins
Para realizar el test con cucumber lo primero que haremos será definir el Jherkin con los test que se van a realizar. Para ello generaremos un *fichero.feature* con la  *feature* y los distintos *escenarios* y la dejaremos en la raiz o alguna carpeta de properties de test.

```
Feature: features of gherkin test
  Scenario Outline: client makes call to GET hello
    When the client named '<name>' calls hello
    Then the client receives status code of 200
    And the client with name '<name>' receives server hello message
    Examples:
      | name |
      | Diego |
      | Sofia |
      | Pepe |
  Scenario Outline: client makes call to GET to sum numbers
    When the client call sum with <a> and <b>
    Then the client receives status code of 200
    And check results of sum correspond with expected result <result>
    Examples:
      | a | b | result |
      | 2 | 3 | 5      |
      | 2 | 4 | 6      |
      | 2 | 5 | 7      |
```

### Dependencias de Maven
Hemos tenigo que incluir en nuestro maven (O gradle según lo utilicéis) las dependencias siguientes:

```xml
<!-- Cucumbder dependencies -->
		<dependency>
			<groupId>io.cucumber</groupId>
			<artifactId>cucumber-java</artifactId>
			<version>6.9.1</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>io.cucumber</groupId>
			<artifactId>cucumber-junit</artifactId>
			<version>6.9.1</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>io.cucumber</groupId>
			<artifactId>cucumber-spring</artifactId>
			<version>6.9.1</version>
			<scope>test</scope>
		</dependency>
```

### Configuración de Cucumber
Para definir las pruebas de cucumber se indica en la siguiente clase la ruta donde se encuentra las *features*, y que corra los test con la clase de Cucumber.

```java
package com.example.demo.cucumber;

import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;
import org.junit.runner.RunWith;

@RunWith(Cucumber.class)
@CucumberOptions(features = "src/test/resources")
public class CucumberIntegrationTest {

}
```

Es necesario además indicar a Cucumber que en su contexto debe levantar el Spring Boot.

```java
package com.example.demo.cucumber;

import com.example.demo.DemoApplication;
import io.cucumber.spring.CucumberContextConfiguration;
import org.springframework.boot.test.context.SpringBootContextLoader;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.ContextConfiguration;

@CucumberContextConfiguration
@SpringBootTest(classes = DemoApplication.class, webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
@ContextConfiguration(classes = DemoApplication.class, loader = SpringBootContextLoader.class)
public class CucumberSpringConfiguration {

}
```

Finalmente en una clase aparte que se instanciará con la inyección de dependencias del Spring previamente levantado se implementarán todas las anotaciones del test de cucumber. Hay que tener en cuentra que si un mismo (When, Then, Given, And...) se repite en una propiedad no hay que implementarlo varias veces sino que se reutilizará.

También hay que tener en cuentra que es posible enviar parámetros definos en los `Examples` de un `Scenario Outline` o bien definido explicitamente con "" comillas al Step tal y como indicamos en el siguiente ejemplo:


foo.feature
```
When the client named '<name>' calls hello
```

FooStep.java
```java
 @When("the client named {string} calls hello")
    public void callHello(String name) {
        responseEntity = testRestTemplate.getForEntity("http://localhost:8080/examples/hello?name=" + name, String.class);
    }
```

Una vez implementado todos los steps, se pueden lanzar los test de todos los escenarios o por separado. Estos test end2end levantarán la aplicacion y el h2 y realizarán una llamada REST  con `TestRestTemplate` emulando el comportamiendo deseado por el consumidor de la aplicación y cumpliendo todos los compartamientos especificados en el fichero Jherkins (que teoricamente redactará el usuario o alguien de negocio).