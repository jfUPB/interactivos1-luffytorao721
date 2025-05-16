## Código Cuadrado

```js
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    fill('white');
    rect(width / 2 - 50, height / 2 - 50, 100, 100); // Dibuja un cuadrado en el centro
}

function draw() {
    // Verifica si hay datos disponibles desde el micro:bit
    if (port.availableBytes() > 0) {
        let dataRx = port.read(1); // Lee un byte de datos
        if (dataRx == 'A') {
            fill('red'); // Cambia el color del cuadrado a rojo
        } else if (dataRx == 'B') {
            fill('yellow'); // Cambia el color del cuadrado a amarillo
        } else {
            fill('green'); // Cambia el color del cuadrado a verde
        }
        background(220); // Limpia el fondo
        rect(width / 2 - 50, height / 2 - 50, 100, 100); // Redibuja el cuadrado
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

### ¿Cuál es el proceso de conexión?


### 1. **Conexión entre el micro:bit y p5.js**
   - **micro:bit**: El micro:bit se programa para enviar datos a través de su puerto serial cuando se presionan los botones A o B.
   - **p5.js**: Usa la biblioteca `p5.webserial` para establecer una conexión serial con el micro:bit y leer los datos enviados.

### 2. **Flujo de Comunicación**
   - Cuando el micro:bit detecta que se presiona un botón, envía un carácter (`A`, `B` o `C`) a través del puerto serial.
   - En p5.js, el programa verifica si hay datos disponibles en el puerto serial. Si los hay, lee el carácter y cambia el color del cuadrado en función del valor recibido:
     - `A`: Cambia el color a rojo.
     - `B`: Cambia el color a amarillo.
     - `C`: Cambia el color a verde.

### 3. **Interfaz en p5.js**
   - **Botón de conexión**: Permite conectar o desconectar el micro:bit. Cuando se conecta, se abre el puerto serial.
   - **Dibujo del cuadrado**: Se dibuja un cuadrado en el centro del lienzo. Su color cambia según los datos recibidos del micro:bit.

### 4. **Biblioteca WebSerial**
   - Para que p5.js pueda comunicarse con el micro:bit, es necesario incluir la biblioteca `p5.webserial` en el archivo `index.html`:
     ```html
     <script src="https://unpkg.com/@gohai/p5.webserial@^1/libraries/p5.webserial.js"></script>
     ```
   - Esta biblioteca permite la comunicación serial entre el navegador y dispositivos como el micro:bit.

### 5. **Pasos para Ejecutar el Proyecto**
   1. **Programar el micro:bit**: se debe Usar MakeCode para cargar el código que envía datos seriales al presionar los botones.
   2. **Conectar el micro:bit**: se conecta al computador mediante USB.
   3. **Ejecutar p5.js**: se abre el programa en un servidor local.
   4. **Conectar en p5.js**: procedemos a hacer clic en el botón "Connect to micro:bit" para establecer la conexión.
   5. **Interactuar**: y por último se presionan los botones A o B en el micro:bit para cambiar el color del cuadrado en p5.js.

