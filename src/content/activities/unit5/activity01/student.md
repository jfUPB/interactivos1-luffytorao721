### **Análisis del Caso de Estudio: Comunicación micro:bit y p5.js**

---

#### **1. Comunicación y Datos Enviados**  
El **micro:bit** envía datos seriales cada 100ms (10Hz) con este formato:  
```python
"xValue,yValue,aState,bState\n"
```  
- **xValue, yValue**: Valores del acelerómetro (entre -1024 y 1024).  
- **aState, bState**: Estado de los botones (`True`/`False`).  
- **`\n`**: Delimitador de fin de mensaje.  

---

#### **2. Estructura del Protocolo ASCII**  
- **Formato**: CSV (valores separados por comas).  
- **Ejemplo concreto**:  
  ```python
  "512,-300,True,False\n"  # Inclinación derecha, botón A presionado
  ```  
- **Importancia del `\n`**: Permite que p5.js identifique cuándo un mensaje está completo usando `readUntil("\n")`.

---

#### **3. Procesamiento en p5.js**  
El sketch transforma los datos en coordenadas y eventos:  

```javascript
// Lectura y transformación de datos
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");  // Lee hasta el salto de línea
  if (data) {
    let values = data.trim().split(",");  // Divide los valores
    if (values.length == 4) {
      // Mapeo de coordenadas (ajuste al centro del canvas)
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      // Conversión de estados
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
    }
  }
}
```

---

#### **4. Generación de Eventos**  
Los eventos se detectan comparando el estado actual y previo de los botones:  

```javascript
function updateButtonStates(newAState, newBState) {
  // Evento "A pressed" (flanco de subida)
  if (newAState && !prevmicroBitAState) {
    lineModuleSize = random(50, 160);  // Cambia tamaño
    print("A pressed");
  }
  
  // Evento "B released" (flanco de bajada)
  if (!newBState && prevmicroBitBState) {
    c = color(random(255), random(255), random(255));  // Cambia color
    print("B released");
  }
  
  // Actualiza estados previos
  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}
```

---

#### **5. Capturas de Dibujos Generados**  
| **Interacción** | **Descripción** |  
|------------------|-----------------|  
|  | Líneas generadas al mantener presionado **A** mientras se inclina el micro:bit. |  
| | Figuras SVG rotando (activadas con teclas **6-9**). |  

---

### **Conclusiones Clave**  
1. **Protocolo eficiente**: El formato CSV + `\n` asegura que los datos lleguen completos.  
2. **Mapeo inteligente**: Los valores del acelerómetro se adaptan al tamaño del canvas.  
3. **Eventos precisos**: La comparación de estados (`prevState` vs `newState`) evita falsos triggers.  

**Ejemplo de salida en consola**:  
```
A pressed  // Al presionar el botón A
B released // Al soltar el botón B
``` 
