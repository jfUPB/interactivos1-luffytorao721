### **Actividad 03: Caso de Estudio - Comunicación Serial con micro:bit**  

#### **🔍 Análisis del Código**  
El código del micro:bit envía datos del acelerómetro (`x`, `y`) y el estado de los botones (`A`, `B`) a través del puerto serial (`UART`) en formato de texto.  

**Estructura del código**:  
```python
data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
uart.write(data)
```  
- **`"{},{},{},{}\n"`**: Crea una cadena con 4 valores separados por comas y un salto de línea (`\n`).  
  - Ejemplo: `"969,652,True,False\n"`.  
- **`uart.write(data)`**: Envía la cadena como bytes a través del puerto serial.  

---

#### **📡 Datos Enviados y su Estructura**  
1. **¿Qué información se envía?**  
   - `xValue`, `yValue`: Valores del acelerómetro (rango: `-1024` a `+1024`).  
   - `aState`, `bState`: `True`/`False` según si los botones A/B están presionados.  

2. **Formato de los datos**:  
   - **Separados por comas**: Para que p5.js pueda interpretar cada valor individualmente (ej: `split(",")`).  
   - **Salto de línea (`\n`)**: Indica el fin de un paquete de datos.  

3. **¿Qué pasa si no se usan comas o `\n`?**  
   - Sin comas: p5.js no podría distinguir entre `xValue` y `yValue`.  
   - Sin `\n`: El programa receptor no sabría cuándo termina un conjunto de datos.  

---

#### **⚙️ Experimentos y Hallazgos**  

| **Pregunta** | **Respuesta** | **Evidencia** |
|--------------|---------------|---------------|
| **¿Para qué sirve `sleep(100)`?** | Limita la frecuencia de envío a **10 Hz** (100 ms). Sin él, el micro:bit saturaría el puerto serial. | Sin `sleep`, SerialTerminal muestra datos demasiado rápido y se pierden. |
| **Valores de `xValue`/`yValue` al inclinar**: | - **Izquierda**: `xValue` negativo. **Derecha**: `xValue` positivo. <br> - **Adelante**: `yValue` negativo. **Atrás**: `yValue` positivo. | Al inclinar, los valores cambian entre `-1024` y `+1024`. |
| **`aState` y `bState`**: | `True` si el botón está presionado; `False` si no. | SerialTerminal muestra `True,False` al presionar solo A. |
| **`was_pressed()` vs `is_pressed()`**: | `was_pressed()` detecta pulsaciones únicas, mientras que `is_pressed()` lee el estado actual. | Con `was_pressed()`, SerialTerminal solo muestra `True` una vez por pulsación. |
| **Bytes enviados para `969,652,True,False`**: | Se envía la cadena como ASCII: <br> `"969,652,True,False\n"` → Hex: `39 36 39 2C 36 35 32 2C 54 72 75 65 2C 46 61 6C 73 65 0A` | Verificado en SerialTerminal en modo **HEX**. |

--- 
1. **El formato CSV + `\n`** es clave para que p5.js interprete los datos correctamente.  
2. **`sleep(100)`** evita sobrecargar la comunicación serial.  
3. **`was_pressed()`** es útil para detectar pulsaciones únicas (ej: inicio de un gesto).

## Puerto serial

![image](https://github.com/user-attachments/assets/eb3cfcfd-5951-4127-b672-b5f2f0820580)

