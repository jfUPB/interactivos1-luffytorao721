``` js
function setup() {
  createCanvas(400, 400);
  noFill();
  stroke(0);
}

function draw() {
  background(255);
  
  let numShapes = 20;  // Número de formas
  let centerX = width / 2;  // Centro de la pantalla
  let centerY = height / 2;
  
  for (let i = 0; i < numShapes; i++) {
    let angle = map(i, 0, numShapes, 0, TWO_PI);  // Ángulo para cada forma
    let offsetX = cos(angle + frameCount * 0.01) * 100;  // Desplazamiento en X
    let offsetY = sin(angle + frameCount * 0.01) * 100;  // Desplazamiento en Y

    // Dibujar una elipse que se mueve de acuerdo a las funciones seno y coseno
    ellipse(centerX + offsetX, centerY + offsetY, random(20, 60), random(20, 60));
  }
}
````
