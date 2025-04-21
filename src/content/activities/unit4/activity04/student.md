

## Sitio: p5.js Examples – Draw Line

**🔗 Enlace del ejemplo original:**  
https://p5js.org/examples/animation-and-variables-drawing-lines/
---

### 1. ¿Qué me llamó la atención?

- **Interactividad libre**: permite dibujar con el mouse de forma espontánea, como si fuera un cuaderno en blanco.  
- **Simplicidad potente**: con muy pocas líneas de código ya tienes un lienzo interactivo ready-to-go.  
- **Posibilidades de personalización**: es muy fácil cambiar grosor, color, mezcla de modos, añadir efectos sonoros, etc.

---

### 2. Análisis del sketch original

- **Funciones principales**:  
  - `setup()` para crear el lienzo (`createCanvas`) y establecer propiedades iniciales (fondo, ancho de trazo).  
  - `draw()` (o `mouseDragged()`) para dibujar continuamente mientras el ratón está presionado.  
- **Interacción**:  
  - `mouseIsPressed` (o `mouseDragged()`) detecta el arrastre.  
  - `pmouseX`, `pmouseY` capturan la posición anterior del ratón para trazar líneas suaves.  
- **Dibujo**:  
  - `stroke()` define el color de línea.  
  - `strokeWeight()` fija el grosor del trazo.  
  - `line(x1, y1, x2, y2)` dibuja la línea desde la posición anterior a la actual.

---

Aquí tienes una versión más creativa de tu sketch “drawLine”, que añade varios elementos nuevos para hacerlo único:

```js
let mode = 0;            // 0 = líneas, 1 = elipses
let hueOffset = 0;       // desplazamiento de color con el tiempo
let fadeAlpha = 20;      // cantidad de desvanecimiento del fondo

function setup() {
  createCanvas(710, 400);
  background(0);
  colorMode(HSB);
  strokeWeight(10);
  noFill();
  describe('Lienzo interactivo con líneas y elipses de colores que cambian con el mouse y el teclado');
}

function draw() {
  // Efecto de fondo que se desvanece lentamente
  push();
  noStroke();
  fill(0, 0, 0, fadeAlpha);
  rect(0, 0, width, height);
  pop();

  // Acentuamos el desplazamiento de color con el tiempo
  hueOffset = (hueOffset + 0.5) % 360;

  if (mouseIsPressed) {
    let speed = dist(pmouseX, pmouseY, mouseX, mouseY);
    let dynamicWeight = map(speed, 0, 50, 5, 30);
    strokeWeight(dynamicWeight);

    // calculamos un color dinámico
    let h = (mouseX - mouseY + hueOffset + 360) % 360;
    stroke(h, 80, 90);

    if (mode === 0) {
      // Línea con ligera variación de ruido para un trazo orgánico
      let nx = noise(frameCount * 0.01) * 5 - 2.5;
      let ny = noise(frameCount * 0.01 + 100) * 5 - 2.5;
      line(pmouseX + nx, pmouseY + ny, mouseX + nx, mouseY + ny);
    } else {
      // Elipses que siguen al cursor
      let d = map(mouseX, 0, width, 10, 100);
      ellipse(mouseX, mouseY, d, d * 0.6);
    }
  }
}

function keyPressed() {
  // Cambiar entre modo línea y modo elipse con la tecla SPACE
  if (key === ' ') {
    mode = 1 - mode;
  }
  // Limpiar pantalla con 'C'
  if (key === 'C' || key === 'c') {
    background(0);
  }
}
```

### ¿Qué hace diferente esta versión?

1. **Fondo con desvanecimiento suave**  
   Cada `draw()` dibuja un rectángulo semitransparente negro, lo que genera un efecto de “desvanecimiento” de los trazos anteriores y crea estelas suaves.

2. **Grosor dinámico según la velocidad del mouse**  
   El grosor del trazo (`strokeWeight`) varía entre 5 y 30 dependiendo de cuán rápido se mueva el ratón, haciendo que movimientos rápidos generen trazos más gruesos y lentos más finos.

3. **Dos modos de dibujo**  
   - **Modo líneas**: líneas con un ligero desplazamiento basado en `noise()` para un acabado más orgánico.  
   - **Modo elipses**: elipses cuyo tamaño depende de la posición del mouse, creando formas más divertidas.  
   Cambias de modo presionando **ESPACIO**.

4. **Control de limpieza**  
   Presionando **C** (o c) limpias el lienzo y vuelves a arrancar con fondo negro.

5. **Color en HSB con desplazamiento continuo**  
   El matiz se va desplazando lentamente con cada frame (`hueOffset`), de modo que incluso si se mantiene el mouse quieto, el color evoluciona con el tiempo.

---
##
**🔗 Enlace de mi versión:**  
https://editor.p5js.org/luffytorao721/sketches/5hZA-aadG
---
