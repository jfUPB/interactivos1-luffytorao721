```js
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');
const app = express();
const server = http.createServer(app);
const io = socketIO(server);
const port = 3000;

let ultimoRitmoBase = null;

// Rutas directas que sirven HTML con p5.js + socket.io + lógica embebida
app.get('/page1', (req, res) => {
    res.send(`
<!DOCTYPE html>
<html>
<head>
    <title>Page 1 - Ritmo base</title>
    <script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.9.0/lib/p5.min.js"></script>
</head>
<body>
    <h1>Page 1 - Marca el ritmo</h1>
    <p>Pulsa cualquier tecla para marcar el ritmo base.</p>
    <script>
        const socket = io();
        let lastTimestamp = 0;
        let circles = [];

        function setup() {
            createCanvas(windowWidth, windowHeight);
            background(30);
        }

        function draw() {
            background(30, 30, 30, 80);
            for (let c of circles) {
                fill(0, 200, 255, 100);
                ellipse(c.x, c.y, c.r * 2);
                c.r += 2;
            }
        }

        function keyPressed() {
            const timestamp = Date.now();
            if (timestamp - lastTimestamp > 300) {
                lastTimestamp = timestamp;
                socket.emit('ritmoBase', { timestamp: timestamp });
                circles.push({ x: random(width), y: random(height), r: 20 });
            }
        }
    </script>
</body>
</html>
    `);
});

app.get('/page2', (req, res) => {
    res.send(`
<!DOCTYPE html>
<html>
<head>
    <title>Page 2 - Responde al ritmo</title>
    <script src="https://cdn.socket.io/4.7.2/socket.io.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/p5@1.9.0/lib/p5.min.js"></script>
</head>
<body>
    <h1>Page 2 - Responde al ritmo</h1>
    <p>Intenta pulsar una tecla justo después de ver el ritmo base.</p>
    <div id="resultado"></div>
    <script>
        const socket = io();
        let lastBase = 0;
        let circles = [];

        socket.on('ritmoBase', (data) => {
            lastBase = data.timestamp;
            circles.push({ x: random(width), y: random(height), r: 10, color: [255, 100, 100, 100] });
        });

        socket.on('resultadoRitmo', (data) => {
            document.getElementById("resultado").innerText =
                data.sincronizado
                    ? "✅ ¡Bien sincronizado! Desfase: " + data.desfase + "ms"
                    : "⚠️ Fuera de ritmo. Desfase: " + data.desfase + "ms";
        });

        function setup() {
            createCanvas(windowWidth, windowHeight);
            background(0);
        }

        function draw() {
            background(0, 0, 0, 80);
            for (let c of circles) {
                fill(c.color);
                ellipse(c.x, c.y, c.r * 2);
                c.r += 2;
            }
        }

        function keyPressed() {
            const now = Date.now();
            if (now - lastBase > 100) {
                socket.emit('ritmoRespuesta', { timestamp: now });
            }
        }
    </script>
</body>
</html>
    `);
});

// Socket.io para sincronizar ritmo base y respuesta
io.on('connection', (socket) => {
    console.log('✅ Usuario conectado - ID:', socket.id);

    socket.on('disconnect', () => {
        console.log('❌ Usuario desconectado - ID:', socket.id);
    });

    socket.on('ritmoBase', (data) => {
        console.log(`🎵 Ritmo base recibido: ${data.timestamp}`);
        ultimoRitmoBase = data.timestamp;
        socket.broadcast.emit('ritmoBase', data);
    });

    socket.on('ritmoRespuesta', (data) => {
        if (ultimoRitmoBase) {
            const desfase = Math.abs(data.timestamp - ultimoRitmoBase);
            const sincronizado = desfase <= 200;
            console.log(`🎯 Ritmo respuesta con desfase: ${desfase}ms`);
            io.emit('resultadoRitmo', { desfase, sincronizado });
        }
    });
});

server.listen(port, () => {
    console.log(`🚀 Servidor en http://localhost:${port}`);
});
```
![image](https://github.com/user-attachments/assets/318c32d6-1643-464f-acb6-04acf63f7c98)

**1. Rol del servidor Node.js**
El server es el punto de encuentro: recibe mensajes de cada navegador y los reenvía. Sin él, los clientes no “ven” al otro porque, por seguridad, los navegadores no se conectan directo entre sí.

**2. `socket.emit()` vs. `socket.broadcast.emit()`**

* `socket.emit()` → envía a **todos**, incluido quien mandó.
* `socket.broadcast.emit()` → envía a **todos menos** al que mandó.
  Uso el primero para actualizaciones globales; el segundo cuando el emisor ya sabe la info y solo hace falta avisar a los demás.

**3. Socket.IO vs. comunicación serial**

* **Socket.IO**: ideal web-a-web; ventaja → mensaje estructurado y bidireccional en tiempo real; desventaja → necesita servidor y conexión Internet/LAN.
* **Serial (ASCII/binario)**: perfecto para microcontroladores; ventaja → simple, sin red; desventaja → solo punto-a-punto y limitado en distancia.

**4. HTTP vs. WebSockets (Socket.IO)**
HTTP sirve los archivos iniciales (HTML, JS, p5). WebSockets toma el relevo para la charla en tiempo real sin recargar página.

**5. Lo que más me sorprendió**
Que con pocas líneas (`io.on`, `socket.emit`) puedo hacer que dos pantallas separadas reaccionen instantáneamente; sentir esa “magia” de interacción en tiempo real fue brutal.
