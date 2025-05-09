### **ASCII vs Binario**

#### **1. Comparación Básica**
| **Aspecto**  | **Texto (ASCII)**                     | **Binario**                        |
|--------------|---------------------------------------|------------------------------------|
| **Ejemplo**  | `"500,300,1,0\n"` (más largo)         | `AA 01 F4 00 01 00 2A` (más corto) |
| **Ventaja**  | Fácil de entender                     | Más rápido y ocupa menos espacio   |
| **Problema** | Se daña si falta una coma o un número | Sin separadores, se desordena fácil|

---

#### **2. ¿Por qué usamos "framing"?**
- **Problema**: A veces el micro:bit manda datos cortados o mezclados.
- **Solución**:  
  - **Byte de inicio** (`0xAA`): Como una bandera que dice "¡empieza aquí!"  
  - **Checksum**: Un número que verifica si los datos llegaron bien (como cuando sumas los dígitos de tu DNI para ver si está correcto).

---

#### **3. Partes Clave del Código**
1. **Leer datos**:
   ```javascript
   // Junta los bytes que llegan
   serialBuffer = serialBuffer.concat(newData);
   ```

2. **Buscar paquetes**:
   ```javascript
   while (serialBuffer.length >= 8) {  // Necesitamos 8 bytes completos
     if (serialBuffer[0] !== 0xAA) {
       serialBuffer.shift(); // Borra bytes basura
       continue; // Sigue buscando
     }
   ```

3. **Verificar datos**:
   ```javascript
   // Suma todos los bytes y compara con el checksum
   let suma = dataBytes.reduce((a, b) => a + b, 0) % 256;
   if (suma !== checksum) {
     console.log("Error: Datos dañados");
   }
   ```

4. **Interpretar valores**:
   ```javascript
   // Convierte bytes en números
   let x = view.getInt16(0);  // Lee 2 bytes como número
   let botonA = view.getUint8(4) === 1; // Lee 1 byte como true/false
   ```

---

#### **4. Errores Comunes**
- **Sin `0xAA`**: El programa no sabe dónde empiezan los datos.  
  *Solución*: Borrar bytes hasta encontrarlo.  
- **Checksum incorrecto**: Los datos vinieron mal.  
  *Solución*: Ignorar ese paquete.  

---

#### **5. ¿Por qué es mejor el binario?**
- **Ejemplo**:  
  - *ASCII*: "500,300,1,0" → 12 letras (12 bytes).  
  - *Binario*: `AA 01 F4 00 01 00 2A` → 7 símbolos (7 bytes).  
- **Resultado**: Menos espacio, más velocidad.

---

#### **6. Analogía Sencilla**
- **ASCII**: Como enviar un mensaje de texto: "Mueve 5, izquierda 3, botón A presionado".  
- **Binario**: Como enviar un emoji secreto: `🚩📏🅰️` (solo quien sabe el código lo entiende).  
