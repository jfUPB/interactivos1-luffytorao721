### **Codigo ejemplo p5.js**
*https://p5js.org/examples/animation-and-variables-conditions/*

### **Actividad 05: Integración de micro:bit con p5.js**  
*https://editor.p5js.org/luffytorao721/sketches/w9oEIeKAE* 

---

#### **2. Código Modificado para micro:bit**  
**Cambios clave**:  
- **Control por acelerómetro**: La posición X/Y del círculo se controla inclinando el micro:bit.  
- **Botones A/B**:  
  - **A**: Cambia el color del círculo.  
  - **B**: Reinicia la posición al centro.  

```javascript
let port, connectBtn;
let microBitConnected = false;
let microBitX, microBitY, microBitAState, microBitBState;

let circleX, circleY;
let circleSize = 50;
let circleColor;

function setup() {
  createCanvas(800, 600);
  circleX = width / 2;
  circleY = height / 2;
  circleColor = color(255, 0, 0);

  // Configuración serial
  port = createSerial();
  connectBtn = createButton("Conectar micro:bit");
  connectBtn.position(10, 10);
  connectBtn.mousePressed(toggleMicrobitConnection);
}

function draw() {
  background(240);

  // Dibujar círculo
  fill(circleColor);
  noStroke();
  ellipse(circleX, circleY, circleSize);

  // Gestión de conexión
  if (!port.opened()) {
    connectBtn.html("Conectar micro:bit");
    microBitConnected = false;
  } else {
    connectBtn.html("Desconectar");
    microBitConnected = true;
    readMicrobitData();
  }
}

function toggleMicrobitConnection() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
  } else {
    port.close();
  }
}

function readMicrobitData() {
  if (port.available() > 0) {
    let data = port.readUntil("\n");
    if (data) {
      let values = data.trim().split(",");
      if (values.length === 4) {
        // Mapear acelerómetro (-1024 a 1024) a coordenadas del canvas
        microBitX = map(int(values[0]), -1024, 1024, 0, width);
        microBitY = map(int(values[1]), -1024, 1024, 0, height);
        microBitAState = values[2] === "true";
        microBitBState = values[3] === "true";

        // Actualizar posición del círculo
        circleX = microBitX;
        circleY = microBitY;

        // Botón A: Cambiar color
        if (microBitAState) {
          circleColor = color(random(255), random(255), random(255));
        }

        // Botón B: Reiniciar posición
        if (microBitBState) {
          circleX = width / 2;
          circleY = height / 2;
        }
      }
    }
  }
}
```

---

#### **3. Enlace al Sketch Modificado**  
**p5.js Editor**: https://editor.p5js.org/luffytorao721/sketches/8j9vPiOb2 

---

#### **4. Capturas de Pantalla**  
| **Estado** | **Imagen** | **Descripción** |  
|------------|------------|-----------------|  
| **Esperando conexión** |![image](https://github.com/user-attachments/assets/a5df949c-3edf-4580-a5ac-fe6b5cdd5e9b)| Botón "Conectar micro:bit" visible. |  
| **Círculo controlado por micro:bit** | ![image](https://github.com/user-attachments/assets/1ea119ad-4cc7-43ee-977e-db342a85acf7)| El círculo se mueve al inclinar el micro:bit. |  
| **Botón A presionado** | | El círculo cambia de color aleatorio. |  

 ```py
from microbit import *
import music

uart.init(baudrate=115200)

# Variables para controlar el cambio de color
color_index = 0
color_sequence = [(255,0,0), (0,255,0), (0,0,255), (255,255,0), (255,0,255)]  # Rojo, Verde, Azul, Amarillo, Magenta

while True:
    x = accelerometer.get_x()
    y = accelerometer.get_y()
    a = button_a.was_pressed()  # Cambiado a was_pressed para detectar pulsación única
    b = button_b.is_pressed()
    
    # Cambio de color con botón A
    if a:
        color_index = (color_index + 1) % len(color_sequence)
        music.play(music.BA_DING)  # Sonido de confirmación
        display.show(Image.HAPPY)  # Feedback visual
        sleep(200)
        display.clear()
    
    # Envía datos incluyendo el componente de color actual
    uart.write("{},{},{},{},{},{},{}\n".format(
        x, 
        y,
        a,
        b,
        color_sequence[color_index][0],  # Componente R
        color_sequence[color_index][1],  # Componente G
        color_sequence[color_index][2]   # Componente B
    ))
    sleep(100)
```
---

### **5. Funcionamiento Detallado**  
1. **Acelerómetro**:  
   - Inclinar el micro:bit **izquierda/derecha** mueve el círculo en el eje X.  
   - Inclinar **adelante/atrás** mueve el círculo en el eje Y.  

2. **Botones**:  
   - **A**: Genera un nuevo color RGB aleatorio.  
   - **B**: Centra el círculo en el canvas.  

3. **Conexión Serial**:  
   - El botón alterna entre conectar/desconectar el micro:bit.  
   - Los datos se leen a **115200 baudios**.  

---

### **6. Posibles Mejoras**  
- **Límites del canvas**: Evitar que el círculo salga del área visible.  
- **Efectos visuales**: Añadir trazo al mover el círculo.  
- **Feedback auditivo**: Sonido al presionar los botones.  
- **Cambio color**: no cambia aveces
--- 

