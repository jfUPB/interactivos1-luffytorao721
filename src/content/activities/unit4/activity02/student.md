
```js
// P_2_3_1_02
//
// Generative Gestaltung – Creative Coding im Web
// ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
// Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
// with contributions by Joey Lee and Niels Poldervaart
// Copyright 2018
//
// http://www.generative-gestaltung.de
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * draw tool. draw with a rotating element (svg file).
 *
 * MOUSE
 * drag                : draw
 *
 * KEYS
 * 1-4                 : switch default colors
 * 5-9                 : switch brush element
 * delete/backspace    : clear screen
 * d                   : reverse direction and mirrow angle
 * space               : new random color
 * arrow left          : rotaion speed -
 * arrow right         : rotaion speed +
 * arrow up            : module size +
 * arrow down          : module size -
 * shift               : limit drawing direction
 * s                   : save png
 */
'use strict';

var c;
var lineModuleSize = 0;
var angle = 0;
var angleSpeed = 1;
var lineModule = [];
var lineModuleIndex = 0;

var clickPosX = 0;
var clickPosY = 0;

function preload() {
  lineModule[1] = loadImage('data/02.svg');
  lineModule[2] = loadImage('data/03.svg');
  lineModule[3] = loadImage('data/04.svg');
  lineModule[4] = loadImage('data/05.svg');
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(255);
  //cursor(CROSS);
  noCursor();
  strokeWeight(0.75);

  c = color(181, 157, 0);
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

function draw() {
  if (mouseIsPressed && mouseButton == LEFT) {
    var x = mouseX;
    var y = mouseY;
    if (keyIsPressed && keyCode == SHIFT) {
      if (abs(clickPosX - x) > abs(clickPosY - y)) {
        y = clickPosY;
      } else {
        x = clickPosX;
      }
    }

    push();
    translate(x, y);
    rotate(radians(angle));
    if (lineModuleIndex != 0) {
      tint(c);
      image(lineModule[lineModuleIndex], 0, 0, lineModuleSize, lineModuleSize);
    } else {
      stroke(c);
      line(0, 0, lineModuleSize, lineModuleSize);
    }
    angle += angleSpeed;
    pop();
  }
}

function mousePressed() {
  // create a new random color and line length
  lineModuleSize = random(50, 160);

  // remember click position
  clickPosX = mouseX;
  clickPosY = mouseY;
}

function keyPressed() {
  if (keyCode == UP_ARROW) lineModuleSize += 5;
  if (keyCode == DOWN_ARROW) lineModuleSize -= 5;
  if (keyCode == LEFT_ARROW) angleSpeed -= 0.5;
  if (keyCode == RIGHT_ARROW) angleSpeed += 0.5;
  
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
  if (keyCode == DELETE || keyCode == BACKSPACE) background(255);

  // reverse direction and mirror angle
  if (key == 'd' || key == 'D') {
    angle += 180;
    angleSpeed *= -1;
  }

  // change color
  if (key == ' ') c = color(random(255), random(255), random(255), random(80, 100));
  // default colors from 1 to 4
  if (key == '1') c = color(181, 157, 0);
  if (key == '2') c = color(0, 130, 164);
  if (key == '3') c = color(87, 35, 129);
  if (key == '4') c = color(197, 0, 123);

  // load svg for line module
  if (key == '5') lineModuleIndex = 0;
  if (key == '6') lineModuleIndex = 1;
  if (key == '7') lineModuleIndex = 2;
  if (key == '8') lineModuleIndex = 3;
  if (key == '9') lineModuleIndex = 4;
}
```

### 1. Variables globales  
- **`c`**: guarda el color actual con el que se va a dibujar o tintar el SVG.  
- **`lineModuleSize`**: controla el tamaño (escala) del elemento que se va a dibujar.  
- **`angle`** y **`angleSpeed`**: llevan la cuenta del ángulo de rotación y la velocidad a la que gira cada elemento.  
- **`lineModule[]`**: array donde se cargan varios archivos SVG (en `preload`).  
- **`lineModuleIndex`**: índice que indica qué SVG (o la opción “línea simple”) está seleccionado.  
- **`clickPosX`**, **`clickPosY`**: memoria de la posición en la que se hizo clic, usada para permitir el “lockeo” horizontal/vertical con Shift.

---

### 2. `preload()`  
Antes de inicializar el sketch, el autor carga cuatro archivos SVG en `lineModule[1]…[4]`. Esto garantiza que las imágenes estén disponibles en memoria al dibujar, sin retrasos.

---

### 3. `setup()`  
- Llama a `createCanvas(windowWidth, windowHeight)` para ocupar toda la ventana.  
- Pinta el fondo de blanco (`background(255)`) y oculta el cursor con `noCursor()` para darle más protagonismo al elemento rotativo.  
- Ajusta el grosor de trazo a `0.75` píxeles (`strokeWeight(0.75)`).  
- Inicializa `c` con un color ocre (`color(181,157,0)`).

---

### 4. `windowResized()`  
Cada vez que la ventana cambia de tamaño, `resizeCanvas` actualiza el lienzo para que siga llenando toda la pantalla.

---

### 5. `draw()`  
Dentro de este bucle continuo:

1. **Detección de dibujo**  
   Solo se ejecuta si el botón izquierdo del ratón está presionado.

2. **Shift‑lock**  
   Si también se mantiene Shift, compara la distancia arrastrada en X y en Y desde el clic inicial y anula el eje con menor desplazamiento para trazar perfectamente horizontal o vertical.

3. **Transformaciones**  
   Con `push()`/`pop()` se traslada el origen al punto de dibujo (`translate(mouseX, mouseY)`) y se gira el sistema de coordenadas según `angle`.

4. **Dibujo condicional**  
   - Si `lineModuleIndex > 0`, pinta el SVG correspondiente usando `tint(c)` y `image(...)` escalado a `lineModuleSize`.  
   - Si `lineModuleIndex == 0`, dibuja una línea diagonal de largo `lineModuleSize` con `stroke(c)` y `line(0,0, lineModuleSize, lineModuleSize)`.

5. **Rotación**  
   Tras dibujar, suma `angleSpeed` al ángulo acumulado para que en el siguiente frame el módulo gire un poco más.

---

### 6. `mousePressed()`  
Al pulsar el ratón:

- Se asigna un tamaño aleatorio a `lineModuleSize` (entre 50 y 160px).  
- Se almacena la posición `(mouseX, mouseY)` en `clickPosX`/`clickPosY` para el comportamiento con Shift.

---

### 7. Manejo de teclado  
- **Flechas Arriba/Abajo**: ajustan `lineModuleSize` incrementalmente.  
- **Flechas Izquierda/Derecha**: modifican `angleSpeed` para acelerar o ralentizar la rotación.  
- **Tecla D**: invierte la dirección de giro y añade 180° al ángulo, creando un efecto espejo.  
- **Espacio**: genera un color aleatorio para `c` con algo de transparencia.  
- **Teclas 1–4**: restauran colores por defecto predefinidos.  
- **Teclas 5–9**: seleccionan si dibujar la línea simple (`5`) o uno de los cuatro SVG (`6`–`9`).  
- **Suprimir/Backspace**: limpia el fondo (vuelve al blanco).  
- **S**: guarda una captura del canvas como PNG.
