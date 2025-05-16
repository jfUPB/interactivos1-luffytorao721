## **Código p5.js para Controlar la Bomba**

```javascript
let port;
let connectBtn;
let isConnected = false;

function setup() {
  createCanvas(300, 200);
  
  // Botones de control
  createButton('A').position(50, 80).mousePressed(() => sendCommand('A'));
  createButton('B').position(100, 80).mousePressed(() => sendCommand('B'));
  createButton('Shake (S)').position(150, 80).mousePressed(() => sendCommand('S'));
  createButton('Touch (T)').position(220, 80).mousePressed(() => sendCommand('T'));
  
  // Conexión serial
  port = createSerial();
  connectBtn = createButton('Conectar a micro:bit');
  connectBtn.position(80, 150);
  connectBtn.mousePressed(toggleConnection);
}

function draw() {
  background(240);
  text("Control Bomba micro:bit", 80, 30);
  
  if (!isConnected) {
    text("Desconectado", 120, 60);
  } else {
    text("Conectado", 120, 60);
  }
}

function toggleConnection() {
  if (!isConnected) {
    port.open('MicroPython', 115200);
    connectBtn.html('Desconectar');
    isConnected = true;
  } else {
    port.close();
    connectBtn.html('Conectar');
    isConnected = false;
  }
}

function sendCommand(cmd) {
  if (isConnected) {
    port.write(cmd);
    console.log("Enviado:", cmd);
  } else {
    alert("¡Conecta primero el micro:bit!");
  }
}
```

### **Cómo Funciona**

1. **Interfaz Simple**:
   - Botones para enviar comandos (`A`, `B`, `S`, `T`).
   - Botón de conexión serial.

2. **Conexión Serial**:
   - Se Usa `createSerial()` de la biblioteca p5.webserial.
   - Baud rate: **115200** (debe coincidir con el código del micro:bit).

3. **Envío de Comandos**:
   - Cada botón envía su letra correspondiente por serial:
     - `A` → Botón A  
     - `B` → Botón B  
     - `S` → Shake  
     - `T` → Touch  


---
