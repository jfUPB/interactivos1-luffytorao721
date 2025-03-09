### **Actividad 5: Leyendo Datos Seriales**



### **Código en Micropython**

```python
from microbit import *

uart.init(baudrate=115200)  # Configura el puerto serial

while True:
    if uart.any():  # Verifica si hay datos disponibles
        data = uart.read(1)  # Lee un byte (carácter) del puerto serial
        display.show(data)   # Muestra el carácter en la matriz de LEDs
```

---

### **Documentación del Proceso**

#### **1. Configuración del Puerto Serial**
- **`uart.init(baudrate=115200)`**: Inicializa el puerto serial con una velocidad de transmisión de **115200 baudios**. Esto permite la comunicación entre el micro:bit y otros dispositivos (como una computadora o un smartphone).

#### **2. Lectura de Datos**
- **`uart.any()`**: Verifica si hay datos disponibles en el puerto serial.
- **`uart.read(1)`**: Lee un solo byte (carácter) del buffer de recepción.

#### **3. Visualización en la Matriz de LEDs**
- **`display.show(data)`**: Muestra el carácter recibido en la matriz de LEDs del micro:bit.

---

### **Prueba del Código**

1. **Conexión del micro:bit**:
   - Conecta el micro:bit a la computadora mediante USB.
   - Usa un programa como **Termite**, **PuTTY** o el **Monitor Serial de Arduino** para enviar datos al micro:bit a través del puerto serial.

2. **Envío de Datos**:
   - Envía caracteres como `A`, `B`, `C`, etc., desde el programa de terminal.
   - El micro:bit recibirá los caracteres y los mostrará en la matriz de LEDs.

---

### **¿Qué arroja?**

- Al enviar el carácter `A`, la matriz mostrará la letra "A".
- Al enviar el carácter `B`, la matriz mostrará la letra "B".
- Si no se envía ningún dato, la matriz permanecerá apagada.

---

### **Análisis**

- El micro:bit puede recibir datos seriales de manera confiable y procesarlos en tiempo real.
- La función `uart.read(1)` es útil para leer datos byte por byte, lo que permite manejar entradas simples como caracteres.
- Este enfoque es ideal para proyectos donde el micro:bit necesita interactuar con otros dispositivos, como computadoras o smartphones.

---



```js
let port;
let connectBtn;

function setup() {
    createCanvas(400, 200);
    background(220);

    // Configura el puerto serial
    port = createSerial();
    connectBtn = createButton('Conectar a micro:bit');
    connectBtn.position(20, 20);
    connectBtn.mousePressed(connectBtnClick);
}

function draw() {
    background(220);

    // Envía 'A' al micro:bit si se presiona la tecla 'A'
    if (keyIsPressed && key === 'A') {
        port.write('A');
    }
    // Envía 'B' al micro:bit si se presiona la tecla 'B'
    if (keyIsPressed && key === 'B') {
        port.write('B');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200); // Abre la conexión serial
        connectBtn.html('Desconectar');
    } else {
        port.close(); // Cierra la conexión serial
        connectBtn.html('Conectar a micro:bit');
    }
}
```

---

### **Cómo Funciona**
- Al presionar la tecla `'A'`, p5.js envía el carácter `'A'` al micro:bit, que lo muestra en la matriz de LEDs.
- Al presionar la tecla `'B'`, p5.js envía el carácter `'B'` al micro:bit, que lo muestra en la matriz de LEDs.

---
