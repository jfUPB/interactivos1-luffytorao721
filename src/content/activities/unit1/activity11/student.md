## Circulo en movimiento 

```js
let port;
let connectBtn;
let circleX = 200; // Posición inicial del círculo en el eje x

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
}

function draw() {
    background(220); // Limpia el fondo en cada frame
    fill('blue');
    noStroke();
    ellipse(circleX, height / 2, 50, 50); // Dibuja el círculo en la posición actual

    // Verifica si hay datos disponibles desde el micro:bit
    if (port.availableBytes() > 0) {
        let dataRx = port.read(1); // Lee un byte de datos
        if (dataRx == 'A') {
            circleX -= 5; // Mueve el círculo a la izquierda
        } else if (dataRx == 'B') {
            circleX += 5; // Mueve el círculo a la derecha
        }
        // Limita el movimiento del círculo dentro del canvas
        circleX = constrain(circleX, 25, width - 25);
    }

    // Actualiza el texto del botón de conexión
    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200); // Abre la conexión serial
    } else {
        port.close(); // Cierra la conexión serial
    }
}
```

## Descripción del Proceso

### 1. **Comunicación entre el micro:bit y p5.js**
   - El micro:bit envía un carácter (`A` o `B`) a través del puerto serial cuando se presionan los botones **A** o **B**.
   - En **p5.js**, el programa lee estos caracteres y ajusta la posición del círculo en el eje **x**:
     - Si recibe `A`, el círculo se mueve **5 píxeles a la izquierda**.
     - Si recibe `B`, el círculo se mueve **5 píxeles a la derecha**.

### 2. **Mapeo de los botones al movimiento**
   - **Botón A**: Envía el carácter `A`, que en p5.js se traduce en una disminución de la coordenada **x** del círculo.
   - **Botón B**: Envía el carácter `B`, que en p5.js se traduce en un aumento de la coordenada **x** del círculo.
   - La función `constrain()` en p5.js asegura que el círculo no se salga de los límites del canvas.

### 3. **Interfaz en p5.js**
   - **Botón de conexión**: Permite conectar o desconectar el micro:bit.
   - **Círculo**: Se dibuja en la posición actual (`circleX`, `height / 2`) y se actualiza su posición en cada frame.


