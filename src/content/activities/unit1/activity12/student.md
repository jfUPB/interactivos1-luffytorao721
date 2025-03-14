## Cargar una imagen



```js
let port;
let connectBtn;
let currentImage;
let images = [];
let imageIndex = 0;

function preload() {
    // Carga las 4 imágenes
    images[0] = loadImage('image1.png');
    images[1] = loadImage('image2.png');
    images[2] = loadImage('image3.png');
    images[3] = loadImage('image4.png');
    currentImage = images[0]; // Establece la imagen inicial
}

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
}

function draw() {
    background(220);
    image(currentImage, 0, 0, width, height); // Muestra la imagen actual

    // Verifica si hay datos disponibles desde el micro:bit
    if (port.availableBytes() > 0) {
        let dataRx = port.read(1); // Lee un byte de datos
        if (dataRx == 'A') {
            imageIndex = 0; // Selecciona la primera imagen
        } else if (dataRx == 'B') {
            imageIndex = 1; // Selecciona la segunda imagen
        } else if (dataRx == 'C') {
            imageIndex = 2; // Selecciona la tercera imagen
        } else if (dataRx == 'D') {
            imageIndex = 3; // Selecciona la cuarta imagen
        }
        currentImage = images[imageIndex]; // Actualiza la imagen actual
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

---

## Código para el micro:bit (MakeCode)

```blocks
basic.forever(function () {
    if (input.buttonIsPressed(Button.A)) {
        serial.writeString("A") // Envía 'A' cuando se presiona el botón A
    } else if (input.buttonIsPressed(Button.B)) {
        serial.writeString("B") // Envía 'B' cuando se presiona el botón B
    } else if (input.isGesture(Gesture.Shake)) {
        serial.writeString("C") // Envía 'C' cuando se agita el micro:bit
    } else if (input.logoIsPressed()) {
        serial.writeString("D") // Envía 'D' cuando se presiona el logo
    }
    basic.pause(100) // Espera 100ms para evitar enviar datos demasiado rápido
})
```

---

## Descripción del Proceso

### 1. **Comunicación entre el micro:bit y p5.js**
   - El micro:bit envía un carácter (`A`, `B`, `C` o `D`) a través del puerto serial cuando se presionan los botones **A** o **B**, se agita el micro:bit o se presiona el logo.
   - En **p5.js**, el programa lee estos caracteres y selecciona la imagen correspondiente:
     - `A`: Muestra la primera imagen.
     - `B`: Muestra la segunda imagen.
     - `C`: Muestra la tercera imagen.
     - `D`: Muestra la cuarta imagen.
    
    Despueés se le asigna una imagen, hasta el momento así como esta. (no carga nada todavia)
    

