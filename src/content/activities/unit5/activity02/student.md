### **Transformación a Protocolo Binario**
---

#### **1. Datos en Texto vs. Hexadecimal**  
**Experimento 1**: Mostrar datos como **Texto**  
- **Resultado**: Caracteres ilegibles (ej: `��`)  
- **Explicación**:  
  El terminal interpreta los bytes binarios como caracteres ASCII, pero al no corresponder a valores imprimibles, muestra símbolos extraños.  

**Experimento 2**: Mostrar datos como **Hexadecimal**  
- **Resultado**: Secuencia de 6 bytes (ej: `01 A4 FF 00 01 00`)  
- **Relación con `struct.pack('>2h2B', ...)`**:  
  - `2h`: 2 enteros cortos (2 bytes cada uno → 4 bytes total para `xValue`, `yValue`).  
  - `2B`: 2 bytes sin signo (1 byte cada uno para `aState`, `bState`).  
  - **Total**: 6 bytes por mensaje.  

---

#### **2. Estructura Binaria**  
**Ejemplo con valores**:  
- Si `xValue = 420` (`01 A4` en hex), `yValue = -256` (`FF 00`), `aState = True` (`01`), `bState = False` (`00`):  
  ```hex
  01 A4 FF 00 01 00
  ```  
- **Números negativos**: Usan complemento a 2 (ej: `-256` → `FF 00`).  

---

#### **3. Ventajas/Desventajas**  
| **Formato Binario**          | **Formato ASCII**          |  
|------------------------------|----------------------------|  
| ✅ **Compacto** (6 bytes vs. ~15 en ASCII) | ❌ Más lento para procesar |  
| ✅ **Más rápido** en transmisión | ✅ Legible por humanos |  
| ❌ **Difícil debug** (requiere conversión) | ❌ Ineficiente para grandes datos |  

---

#### **4. Experimento con Gestos**  
**Código modificado**:  
```python
if accelerometer.was_gesture('shake'):
    data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
    uart.write(data)
```  
**Hallazgos**:  
- Se envían **6 bytes exactos** por shake (confirmado con el terminal hexadecimal).  
- **Estructura**:  
  - Bytes 1-2: `xValue` (ej: `01 A4` = 420).  
  - Bytes 3-4: `yValue` (ej: `FF 00` = -256).  
  - Byte 5: `aState` (`01` = `True`).  
  - Byte 6: `bState` (`00` = `False`).  

---

#### **5. Comparación ASCII vs. Binario**  
**Datos ASCII**:  
```
"420,-256,True,False\n"  # 19 bytes
```  
**Datos Binarios**:  
```
01 A4 FF 00 01 00         # 6 bytes
```  
**Key Insights**:  
- **Binario reduce un 68% el ancho de banda**.  
- **ASCII es útil para debugging rápido**, pero ineficiente en producción.  

---

### **Conclusiones**  
1. **Protocolo binario** optimiza velocidad/espacio, ideal para aplicaciones en tiempo real.  
2. **ASCII** sigue siendo relevante para prototipado y logs.  
3. **Herramientas clave**:  
   - `struct.pack()` para empaquetado binario.  
   - Terminal hexadecimal para depuración.  

**Capturas de referencia**:  
# Importante_montar

**Próximos pasos**: Implementar el parser binario en p5.js usando `DataView`.  

--- 
