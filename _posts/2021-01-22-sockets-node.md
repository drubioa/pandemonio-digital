---
layout: post
title:  "Demo Socket.io + Node"
date:   2021-01-22 12:15:00 +0100
categories: demo
author: Diego Rubio Abujas
tags: node demo
---

![cabecera]({{ site.url }}{{ site.baseurl }}/assets/images/demo-socket-node/node+socket-io.png){:class="page.image"}

[Repositorio GitHub](https://github.com/drubioa/demo-node-mailbox-notification)

# Introducción
En esta ocasión nos metemos a pelearnos con dos cosas que llevo unos días ojeando y que paso a resumir/explicar a continuación:

## WebSocket 
De un tiempo atrás quiero trastear un poco el tema de las conexiones con `websocket`, ya hace años en un proyecto en que me encontraba propuse solucionar una problemática semejante a la que planteo en esta prueba de concepto. La idea es disponer de una conexión abierta entre un cliente y un servidor, de forma que si en cualquier momento cambia una situación en el servidor, el cliente se entere de manera inmediata sin tener que estar llamando al servidor cada X tiempo.

Pero vamos a explicar un poco en qué consiste todo esto de los `websocket`, y para ello nos agenciamos la definición de Wikipedia:

>WebSocket es una tecnología que proporciona un canal de comunicación bidireccional y full-duplex sobre un único socket TCP. Está diseñada para ser implementada en navegadores y servidores web, pero puede utilizarse por cualquier aplicación cliente/servidor. La API de WebSocket está siendo normalizada por el W3C, mientras que el protocolo WebSocket ya fue normalizado por la IETF como el RFC 6455. Debido a que las conexiones TCP comunes sobre puertos diferentes al 80 son habitualmente bloqueadas por los administradores de redes, el uso de esta tecnología proporcionaría una solución a este tipo de limitaciones proveyendo una funcionalidad similar a la apertura de varias conexiones en distintos puertos, pero multiplexando diferentes servicios WebSocket sobre un único puerto TCP (a costa de una pequeña sobrecarga del protocolo).
>
> --Wikipedia

Cuando realizamos una petición HTTP utilizamos un protocolo de conexión que trabaja a más alto nivel que cuando realizamos una conexión `socket` mediante protocolo websockets. Este protocolo es similar a los sockets de UNIX pero funciona en la web y posibilita trasnaferencia de datos en tiempo real y de manera bidireccional. A diferencia de HTTP las conexion de WebSocket no tienen cabeceras, los mensajes suelen tener menor tamaño y no gestiona recursos en URLs.

Socket TCP al trabajar a muy bajo nivel permite conexiones mucho mas rápidas tanto de forma síncrona como asíncrona. Una característica muy interesante de las conexiones por WebSocket es que al contrario que las peticiones HTTP que cierran la conexión al obtener la respuesta, estas al ser bidireccionales permanecen abiertas hasta que uno de los dos corta la conexión, y al trabajar a tan bajo nivel no suponen muchos recursos ni de red, ni por parte del servidor.

## Node
Aunque en un futuro pienso realizar esta misma prueba de concepto en java, hoy vengo con node en el lado del servidor. Recientemente a raíz de un curso de Amazon AWS Serverless que estoy realizando he visto que para Serverless es utiliza mucho node en la parte back, lo que asu vez me ha llevado a realizar un curso de Node en el cual se tocaba bastante el tema de los socket, y ha sido lo que me ha llevado a escribir este artículo.

>Node.js es un entorno en tiempo de ejecución multiplataforma, de código abierto, para la capa del servidor (pero no limitándose a ello) basado en el lenguaje de programación JavaScript, asíncrono, con E/S de datos en una arquitectura orientada a eventos y basado en el motor V8 de Google. Fue creado con el enfoque de ser útil en la creación de programas de red altamente escalables, como por ejemplo, servidores web.4​ Fue creado por Ryan Dahl en 2009 y su evolución está apadrinada por la empresa Joyent, que además tiene contratado a Dahl en plantilla.
>
> --Wikipedia

Como curiosidad y basándome en distintos artículos que he leído decir que **node** permite despliegues muy rápidos en contenedores muy ligeros. El hecho de poder utilizar typescript está muy interesante, y el framework [express](https://expressjs.com/es/) permite de manera muy sencilla y elegante crear nuestro microservicios. 

También decir que el [npm](https://www.npmjs.com/) dispone de muchísimas librerías y soluciones a problemas que nos surjan en nuestro desarrollo. Y aunque no lo he probado he leído que soporta bastante bien una cantidad elevada de peticiones simultáneas.

## Socket.io

Para la prueba que nos disponemos hacer hemos utilizado tanto el el lado del cliente (en la web y navegador del cliente), como en el servidor la librería [Socket.io](https://socket.io/).

>Socket.IO es una biblioteca de JavaScript para aplicaciones web en tiempo real. Permite la comunicación bidireccional en tiempo real entre clientes y servidores web. Tiene dos partes: una biblioteca del lado del cliente que se ejecuta en el navegador y una biblioteca del lado del servidor para Node.js.
>
> --Wikipedia

Esta librería de javascript nos permitirá establecer las comunicaciones vía socket entre cliente y servidor. Pose conexiones abiertas entre cliente y servidor que posibilitan que cuando haya un cambio en el servidor, éste se refleje de manera inmediata en el cliente como veremos a continuación. Ademas por lo que hemos probado permite autentificar e identificar las distintas conexiones que se dejan abiertas entre cliente y servidor. Su web goza de una documentación muy rica, llena de ejemplos y aclaraciones. Como curiosidad decir que entre las empresas que lo usan, casi todo lo que veo son casinos.

# La prueba de concepto
Pues la prueba que planteamos es la siguiente: queremos simular una aplicacion web que gestiona correspondencia de un usuario. Inicialmente al entrar en la web el buzón aparece sin mensajes, pero el servidor tiene un endpoint que al recibir peticiones POST con el nombre del usuario debe notificar al usuario que tiene un mensaje sin leer. Y vamos un poco más, si recibe varias peticiones este número de peticiones se va incrementando.

Aunque la prueba se plantea para un solo usuario, debe darse cierta lógica para que si mañana nos llegasen varios usuarios se llevara la cuenta de los mensajes pendientes de leer que tiene cada uno de ellos. 

El siguiente diagrama muestra resumidamente lo que pretendemos hacer:

![diagrama]({{ site.url }}{{ site.baseurl }}/assets/images/demo-socket-node/diagram.jpg){:class="page.image"}

## Implementando 

El primer paso tras `npm init` fue instalar las dependencias que indicamos a continuación:

```json
    "dependencies": {
        "express": "^4.17.1",
        "socket.io": "^3.1.0"
    }
```

Una vez hemos instalado todas las dependencias vamos a explicar un poco cómo hemos estructurado la aplicación:

La aplicación va a tener dos partes:

### Cliente

La parte de **cliente** que se encuentra en la carpeta `\public` y que consta de un fichero `index.html` con las dependencias de `SocketIO` y `index.js`. En este último fichero de script tenemos la conexión que realiza el cliente con el socket como podemos ver a continuación:

```javascript
let socket = io({ 
    auth: {
        token: "P@sword" // Al instanciar socket incluimos un token con una contraseña 
    },
    query: {
        username: 'anonymous' // Podemos pasar parámetros al establecer la conexión 
    }
});
let unreadMessages;

// Mediante on estamos a la escucha, cuando nos conectemos al servidor se mostrará el identificador de la conexión
socket.on('connect', () => {  
    console.log('Connected to server with id: ' + socket.id); 
    document.getElementById("unreadMessages").innerHTML = unreadMessages ? unreadMessages : 0;
});

socket.on('unreadMessage', (unreadMessages) => {
    console.log('unread mesages: ' + unreadMessages);
    // Cuando el servidor nos comunique via socket que hay un nuevo mensaje lo mostraremos el html
    document.getElementById("unreadMessages").innerHTML = unreadMessages ? unreadMessages : 0; 
});
```

### Servidor

Por otra parte en el servidor hemos definido dos clases:

1. **unreadMessagesNotificationService**: un servicio en el que mediante un *Map* guardaremos información sobre los mensajes pendiente de leer que tiene cada usuario que se registre en nuestra aplicación mediante la conexión socket.

```javascript
const app = require('express')();
const http = require('http').Server(app);

let unreadMessagesMap = new Map();

// Cuando un usuario recibe un mensaje incrementamos el numero de mensajes que tenga en uno
exports.newMessage = (username) => {

    if (!unreadMessagesMap.get(username)) {
        throw new Error(`Cannot obtains username ${username}`);
    }
    unreadMessagesMap.get(username).pendingToRead = unreadMessagesMap.get(username).pendingToRead + 1;
};

exports.getPendingToReadMessages = (username) => {

    return unreadMessagesMap.get(username);
};

// Cuando un usuario se desconecte lo eliminamos del Map
exports.removeMesageNotification = (socket) => {

    const username = socket.request._query['username'];
    if (unreadMessagesMap.delete(username)) {
        console.log(`removes of unreadMessagesMap usename: ${username}`);
    } else {
        console.log(`Cannot remove of unreadMessagesMap usename: ${username}`);
    }

};

// Cuando un usuario se conecte lo incluimos en el Map
exports.addMesageNotification = (socket) => {
    const username = socket.request._query['username'];
    console.log(`Add message notification to username:  ${username}`);

    if (!unreadMessagesMap.get(username)) {
        unreadMessagesMap.set(username, {
            socketId: socket.id,
            pendingToRead: 0
        });
    }
};
```

2. Y por otra parte tenemos nuestro `server.js` con el **endpoint** para notificar mediante REST los nuevos mensajes, y la configuración del **socket**:

```javascript
const express = require('express');
const app = require('express')();
const http = require('http').Server(app);
const io = require('socket.io')(http);

const port = process.env.PORT || 8085;
const unreadMessagesNotificationService = require('./service/unreadMessagesNotificationService');
app.use(express.static('public'));

// Nos valemos de un listado en el que iremos registrando las conexiones socket
let registeredSockets = [];

// Endpoint to notify new messages
app.post('/new-message', (req, res) => {
    let username = req.query.username;
    console.log(`username: ${username}`);
    unreadMessagesNotificationService.newMessage(username);
    const data = unreadMessagesNotificationService.getPendingToReadMessages(username);
    // mediante emit enviamos un mensaje a la conexion registrada previamente indicando el número de mensajes que tiene pendientes de leer
    registeredSockets[data.socketId].emit('unreadMessage', data.pendingToRead);
    res.status(200);
    res.json({
        unreadMessages: data.pendingToRead
    });
});

io.on('connect', (socket) => {

    // Este evento se produce cada vez que el usuario se conecta
    console.log('a user connected');
    registeredSockets[socket.id] = socket;

    io.use((socket, next) => {
        unreadMessagesNotificationService.addMesageNotification(socket);
        next();
    });

    // Token validation example
    io.use((socket, next) => _validateToken(socket, next));

    // Cuando se desconecta eliminamos del listado de socket y del Map del service la información de este usuario
    socket.on('disconnect', () => {
        unreadMessagesNotificationService.removeMesageNotification(socket);
        registeredSockets.splice(registeredSockets.indexOf(socket.id), 1);
        console.log('user disconnected');
    });

});

http.listen(port, () => {
    console.log(`listening on: ${port}`);
});

// Una validación de ejemplo
const _validateToken = (socket, next) => {
    const token = socket.handshake.auth.token;
    const username = socket.request._query['username'];
    console.log(username);
    if (token == 'P@sword') {
        console.log('Valid password');
        next();
    } else {
        console.log('Invalid password');
        next(new Error("invalid password"));
    }
};
```

> Quizás demasiado código javascript en este post; pero quería explicar un poco cómo he implementado esta prueba de concepto mediante los comentarios que he ido añadiendo en las distintas clases del proyecto.

## Resultado

![demo]({{ site.url }}{{ site.baseurl }}/assets/images/demo-socket-node/demo-mailbox-socket.gif){:class="page.image"}

Como podemos observar una vez levantada la aplicación tenemos por un lado en nuestro navegador la web con 0 mensajes sin leer, si procedemos a realizar una llamada POST al servidor e incluimos un nuevo mensaje para ese usuario observamos como de manera inmediata se incrementa el número de mensajes sin leer. Conviene observar el log tanto de la parte node como del cliente, para observar como se sucede la conexión vía socket.