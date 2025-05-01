### **Bitácora - Consolidación de Conceptos**

#### **1. Protocolo de comunicación**  
Es un conjunto de reglas para intercambiar datos. En comunicación serial es esencial porque:  
- Define cómo se estructuran los datos  
- Permite que ambos dispositivos (micro:bit y p5.js) interpreten correctamente la información  
```python
# En micro:bit (formato: x,y,A,B\n)
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
```

#### **2. Separación por comas**  
Las comas actúan como delimitadores para:  
- Identificar valores individuales en una cadena  
- Permitir el uso de `split()` en p5.js  
```javascript
// En p5.js:
let values = data.trim().split(",");  // Divide "123,456,true,false" en array
```

#### **3. Carácter de fin de mensaje (\n)**  
Es necesario porque:  
- Indica cuándo un paquete de datos está completo  
- Permite usar `readUntil("\n")` para lectura sincronizada  
```javascript
let data = port.readUntil("\n");  // Lee hasta encontrar salto de línea
```

#### **4. Máquina de estados**  
Se usó para manejar:  
- Conexión/desconexión del micro:bit  
- Transiciones entre modos de operación  
```javascript
const STATES = {
  WAIT_CONNECTION: "WAIT",
  DRAWING: "DRAW"
};
```

#### **5. Formateo en micro:bit**  
Los datos se convierten a string con formato fijo:  
```python
uart.write("{},{},{},{}\n".format(x, y, a, b))
```

#### **6. Codificación ASCII**  
Significa que los números se envían como caracteres (ej: 123 → "1","2","3"), no como valores binarios.  
```python
# El valor 123 se envía como tres bytes: 49, 50, 51 (ASCII de '1','2','3')
```

#### **7. Verificación de bytes disponibles**  
Evita bloquear el programa si no hay datos:  
```javascript
if (port.availableBytes() > 0) {  // Solo leer si hay datos
  let data = port.readUntil("\n");
}
```
**Sin esto**: El programa podría congelarse esperando datos.

#### **8. Eliminar retorno de carro**  
Se usa `trim()`:  
```javascript
data = data.trim();  // Elimina \n y espacios
```

#### **9. División por espacios**  
Se usaría `split()` con espacio como delimitador:  
```javascript
let partes = cadena.split(" ");  // Para "hola mundo" → ["hola", "mundo"]
```

#### **10. Conversión a números**  
Los datos llegan como texto (ej: "123"), pero necesitamos valores numéricos para cálculos:  
```javascript
microBitX = int(values[0]);  // Convierte "123" → 123
```

#### **11. Bytes enviados**  
Para `123,756,False,True\n` se enviarían (en hexadecimal):  
```
31 32 33 2C 37 35 36 2C 46 61 6C 73 65 2C 54 72 75 65 0A
```
*(ASCII de: 1,2,3,,,7,5,6,,F,a,l,s,e,,T,r,u,e,\n)*

#### **12. Aprendizaje micro:bit**  
- Uso de `uart.init()` para configuración serial  
- Envío continuo de datos con `while True`  
```python
uart.init(baudrate=115200)
while True:
    uart.write(data)
    sleep(100)
```

#### **13. Aprendizaje p5.js**  
- Uso de `createSerial()` para comunicación serial en navegador  
- Mapeo de valores con `map()` para coordenadas:  
```javascript
x = map(valor, -1024, 1024, 0, width);  // Adapta rango del acelerómetro
```

--- 

**Reflexión final**:  
Esta unidad me ayudó a entender cómo diseñar protocolos simples pero robustos para comunicación entre dispositivos, y la importancia de la sincronización en sistemas embebidos. Los conceptos de máquinas de estados y parsing de datos son aplicables a muchos otros proyectos.  

