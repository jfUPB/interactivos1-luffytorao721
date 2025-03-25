# Actividad 07: Crear la Bomba en p5.js

## Código para p5.js (Visualización de la Bomba)

```javascript
let bombState = "CONFIGURATION"; // Estados: CONFIGURATION, ARMED, EXPLOSION
let countdownTime = 20;
let remainingTime = 0;
let lastUpdate = 0;
let sequence = [];
const correctSequence = ["A", "B", "A", "S"];

function setup() {
  createCanvas(400, 400);
  textSize(32);
  textAlign(CENTER, CENTER);
}

function draw() {
  background(240);
  
  // Mostrar estado actual
  fill(0);
  text(`Estado: ${bombState}`, width/2, 50);
  
  // Mostrar tiempo/configuración
  if (bombState === "CONFIGURATION") {
    text(`Tiempo: ${countdownTime}s`, width/2, height/2);
    text("Usa A/B para ajustar", width/2, height/2 + 50);
    text("Agita para armar", width/2, height/2 + 100);
  } 
  else if (bombState === "ARMED") {
    text(`Tiempo restante: ${remainingTime}s`, width/2, height/2);
    
    // Parpadeo en últimos 5 segundos
    if (remainingTime <= 5 && millis() % 1000 < 500) {
      fill(255, 0, 0);
      text(`¡${remainingTime}s!`, width/2, height/2 + 50);
    }
    
    // Mostrar secuencia ingresada
    text(`Secuencia: ${sequence.join("-")}`, width/2, height/2 + 100);
  } 
  else if (bombState === "EXPLOSION") {
    // Animación de explosión
    if (millis() - lastUpdate < 2000) {
      fill(255, 0, 0);
      textSize(64);
      text("💥", width/2, height/2);
      textSize(32);
    } else {
      resetBomb();
    }
  }
  
  // Actualizar tiempo si está armada
  if (bombState === "ARMED") {
    updateCountdown();
  }
}

function updateCountdown() {
  if (millis() - lastUpdate >= 1000) {
    remainingTime--;
    lastUpdate = millis();
    
    if (remainingTime <= 0) {
      bombState = "EXPLOSION";
      lastUpdate = millis();
    }
  }
}

function resetBomb() {
  bombState = "CONFIGURATION";
  countdownTime = 20;
  sequence = [];
}

function keyPressed() {
  if (bombState === "CONFIGURATION") {
    if (key === 'a' || key === 'A') {
      countdownTime = min(60, countdownTime + 1);
    } else if (key === 'b' || key === 'B') {
      countdownTime = max(10, countdownTime - 1);
    } else if (key === 's' || key === 'S') {
      armBomb();
    }
  } else if (bombState === "ARMED") {
    if (key === 'a' || key === 'A') {
      sequence.push("A");
    } else if (key === 'b' || key === 'B') {
      sequence.push("B");
    } else if (key === 's' || key === 'S') {
      sequence.push("S");
    }
    
    checkSequence();
  }
}

function armBomb() {
  bombState = "ARMED";
  remainingTime = countdownTime;
  lastUpdate = millis();
  sequence = [];
}

function checkSequence() {
  if (sequence.length >= correctSequence.length) {
    const recentSequence = sequence.slice(-correctSequence.length);
    if (JSON.stringify(recentSequence) === JSON.stringify(correctSequence)) {
      resetBomb();
    }
  }
}
```

## Características implementadas:

1. **Tres estados de la bomba**:
   - CONFIGURATION: Ajuste del tiempo inicial (10-60 segundos)
   - ARMED: Cuenta regresiva activa
   - EXPLOSION: Animación de explosión

2. **Controles por teclado**:
   - A/B: Ajustar tiempo (en configuración) o ingresar secuencia (en modo armado)
   - S: Armar la bomba (en configuración) o parte de la secuencia (en modo armado)

3. **Secuencia de desactivación**:
   - A-B-A-S (mostrada en pantalla)

