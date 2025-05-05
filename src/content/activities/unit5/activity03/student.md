### * Implementación de Protocolo Binario **

-----

![20250505_114448](https://github.com/user-attachments/assets/f487d390-349d-40ae-9bdf-087ebf0de3f8)





**Problema inicial con ASCII:**
En el protocolo anterior usábamos texto plano con:
- **Comas** para separar valores (ej: `"512,284,True,False\n"`)
- **Salto de línea** (`\n`) para marcar el fin del mensaje

**Limitaciones:**
- Datos de tamaño variable (ej: "-1024" ocupa más bytes que "0")
- Procesamiento lento por conversiones de texto
- Frágil ante errores de transmisión

**Solución con protocolo binario:**
```python
# Código micro:bit
data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
```
- **2h**: 2 enteros cortos (16 bits) para coordenadas
- **2B**: 2 bytes sin signo para estados de botones
- **Tamaño fijo**: 6 bytes por mensaje

**Problema detectado:**
- **Desincronización**: Si p5.js lee mal un byte, todos los valores siguientes se corrompen
- **Ejemplo en consola**:
  ```
  microBitX: 512 microBitY: 284  # Correcto
  microBitX: 1280 microBitY: 3   # Error por desfase
  ```

**Mejora implementada - Framing:**
```python
# Paquete de 8 bytes:
# [0xAA][Datos(6B)][Checksum]
packet = b'\xAA' + data + bytes([sum(data) % 256])
```

**Cambios clave en p5.js:**
1. **Buffer acumulativo**:
   ```javascript
   let serialBuffer = [];
   ```
2. **Detección de paquetes**:
   ```javascript
   while (serialBuffer.length >= 8) {
     if (serialBuffer[0] !== 0xAA) {
       serialBuffer.shift(); // Descarta bytes basura
       continue;
     }
   ```
3. **Validación con checksum**:
   ```javascript
   let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;
   if (computedChecksum !== receivedChecksum) {
     console.log("Checksum error");
   }
   ```

**Resultados:**
- **Consola limpia**:
  ```
  microBitX: 512 microBitY: 284 microBitAState: true
  microBitX: 514 microBitY: 281 microBitAState: false
  ```
- **Errores manejados**:
  ```
  Checksum error in packet [Descarta paquetes corruptos]
  ```

**Comparación final**:
| **Aspecto**       | **Protocolo ASCII**       | **Protocolo Binario**      |
|--------------------|---------------------------|----------------------------|
| Tamaño mensaje     | ~15-20 bytes              | 8 bytes (6 datos + 2 control) |
| Velocidad          | Lento (parsing de texto)  | Rápido (lectura directa)   |
| Robustez           | Frágil (depende de `\n`)  | Sincronización automática  |
| Debugging          | Legible                   | Requiere conversión hex    |

**Conclusión**:  
El nuevo sistema resuelve los problemas de desincronización mediante:
1. **Marcador claro** (0xAA) para identificar el inicio de paquete
2. **Verificación de integridad** con checksum
3. **Tamaño fijo** que elimina ambigüedades

**Próximos pasos**:  
- Implementar tipos de mensaje (ej: 0xAA=datos, 0xBB=configuración)
- Añadir compresión para reducir aún más el tamaño de paquetes

**Reflexión personal**:  
Aunque el protocolo binario requiere más trabajo inicial, los beneficios en velocidad y confiabilidad son esenciales para aplicaciones en tiempo real como videojuegos o instalaciones interactivas. El sistema ahora es capaz de recuperarse automáticamente de errores de transmisión, 
algo crítico en entornos profesionales.
