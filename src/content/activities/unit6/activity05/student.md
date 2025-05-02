### **Implementación **

#### **1. Modificaciones en `server.js`**
```js
const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);
const path = require('path');

app.use(express.static('views'));

// Nuevas rutas para 4 jugadores
app.get('/page1', (req, res) => res.sendFile(path.join(__dirname, 'views/page1.html')));
app.get('/page2', (req, res) => res.sendFile(path.join(__dirname, 'views/page2.html')));
app.get('/page3', (req, res) => res.sendFile(path.join(__dirname, 'views/page3.html')));
app.get('/page4', (req, res) => res.sendFile(path.join(__dirname, 'views/page4.html')));

// Estado del juego
const gameState = {
  drawings: [],
  players: {},
  chatMessages: []
};

io.on('connection', (socket) => {
  console.log('Usuario conectado:', socket.id);
  
  // Asignar color aleatorio al jugador
  const playerColor = `#${Math.floor(Math.random()*16777215).toString(16)}`;
  gameState.players[socket.id] = { color: playerColor };
  
  // Enviar estado inicial al nuevo jugador
  socket.emit('init', { 
    drawings: gameState.drawings,
    players: gameState.players,
    chat: gameState.chatMessages
  });

  // Eventos principales
  socket.on('draw', (data) => {
    const drawing = { ...data, playerId: socket.id };
    gameState.drawings.push(drawing);
    socket.broadcast.emit('newDrawing', drawing);
  });

  socket.on('chatMessage', (msg) => {
    const chatMsg = { text: msg, playerId: socket.id, timestamp: Date.now() };
    gameState.chatMessages.push(chatMsg);
    io.emit('newMessage', chatMsg);
  });

  socket.on('disconnect', () => {
    delete gameState.players[socket.id];
    io.emit('playerDisconnected', socket.id);
  });
});

http.listen(3000, () => console.log('Servidor en http://localhost:3000'));
```

#### **2. Ejemplo de `page1.js` **
```js
const socket = io();
let currentColor = '#FF0000';
let isDrawing = false;
let lastPos = { x: 0, y: 0 };

// Configuración inicial del lienzo
function setup() {
  const canvas = createCanvas(800, 600);
  canvas.parent('canvas-container');
  background(255);
  
  // Eventos de Socket.IO
  socket.on('init', (data) => {
    // Dibujar historial existente
    data.drawings.forEach(draw => {
      stroke(data.players[draw.playerId].color);
      line(draw.x0, draw.y0, draw.x1, draw.y1);
    });
  });

  socket.on('newDrawing', (data) => {
    stroke(gameState.players[data.playerId].color);
    line(data.x0, data.y0, data.x1, data.y1);
  });

  socket.on('newMessage', (msg) => {
    const chat = document.getElementById('chat');
    chat.innerHTML += `<p style="color: ${gameState.players[msg.playerId].color}">${msg.text}</p>`;
  });
}

// Lógica de dibujo
function mousePressed() {
  isDrawing = true;
  lastPos = { x: mouseX, y: mouseY };
  return false;
}

function mouseDragged() {
  if (isDrawing) {
    stroke(currentColor);
    line(lastPos.x, lastPos.y, mouseX, mouseY);
    
    socket.emit('draw', {
      x0: lastPos.x,
      y0: lastPos.y,
      x1: mouseX,
      y1: mouseY
    });
    
    lastPos = { x: mouseX, y: mouseY };
  }
  return false;
}

function mouseReleased() {
  isDrawing = false;
}

// Chat
document.getElementById('chat-form').addEventListener('submit', (e) => {
  e.preventDefault();
  const input = document.getElementById('chat-input');
  socket.emit('chatMessage', input.value);
  input.value = '';
});
```

#### **3. Nuevo `page1.html`**
```html
<!DOCTYPE html>
<html>
<head>
  <title>Pixel War - Jugador 1</title>
  <style>
    body { display: flex; font-family: sans-serif; }
    #canvas-container { margin-right: 20px; }
    #chat-container { width: 300px; border-left: 1px solid #ccc; padding: 10px; }
    #chat-messages { height: 500px; overflow-y: auto; }
  </style>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
</head>
<body>
  <div id="canvas-container"></div>
  
  <div id="chat-container">
    <h2>Chat</h2>
    <div id="chat-messages"></div>
    <form id="chat-form">
      <input id="chat-input" type="text" placeholder="Escribe un mensaje...">
      <button type="submit">Enviar</button>
    </form>
  </div>

  <script src="/socket.io/socket.io.js"></script>
  <script src="page1.js"></script>
</body>
</html>




