## Preguntas y respuestas

#### *Describe qué pasa en el punto 15 y cómo crees que esto se logre.*
- Al momento de oprimir los botones A y B cambia el color de un circulo en la pantalla e intercala entre rojo y amarillo

``` js 
  if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');
        }
        else if(dataRx == 'B'){
            fill('yellow');
  ```
Se hace uso de un condicional en donde  se definen unas entradas que se encuentran el microbit, siel usuario presiona A se lee los datos y arroja una salida, en este caso un color, que es el rojo
si se oprime B entonces se rellena por el color amarillo.

#### *Describe qué pasa en el punto 16 y cómo crees que esto se logre.*
- Al momento de Sacudir el microbit el circulo que se encontraba en el canvas pasará a ser verde.
``` js
else{
            fill('green');
        }
        background(220);
        ellipse(width / 2, height / 2, 100, 100);
        fill('black');
        text(dataRx, width / 2, height / 2);
```
Creo que esto se logra debido a que en el microbit el acelerometro es por así decirlo un botón c y como se logra observar en el código si recibe otra entrada diferente de A y B entonces se rellenará de verde.

#### *Describe qué pasa en el punto 17 y cómo crees que esto se logre.*
- Al oprimir el botón "Send Love" en el microbit se logra ver una carita feliz

``` js
 connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
    fill('white');
    ellipse(width / 2, height / 2, 100, 100);
```
Se crea el boton Send love al oprimirlo se almacena en memoria y cuando se llama se ejecuta una función que hace hace una cara feliz y listo. 
