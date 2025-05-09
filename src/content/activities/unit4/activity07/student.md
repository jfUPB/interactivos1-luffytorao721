### **Autoevaluación - Unidad de Comunicación Serial entre micro:bit y p5.js**  

#### **1. ¿Qué aprendí en esta unidad?**  
- **Protocolos de comunicación serial**: Entendí cómo estructurar datos (formato CSV + `\n`) para que p5.js y micro:bit se comuniquen eficientemente.  
  ```python
  # Código micro:bit (formato: x,y,A,B\n)
  data = "{},{},{},{}\n".format(x, y, a, b)
  ```
- **Máquinas de estados**: Aprendí a gestionar flujos complejos (ej: conexión, dibujo, desconexión).  
  ```javascript
  const STATES = { WAIT_CONNECTION: "WAIT", RUNNING: "RUN" };
  ```
- **Mapeo de valores**: Convertir datos del acelerómetro (-1024 a 1024) a coordenadas de pantalla.  
  ```javascript
  x = map(valor, -1024, 1024, 0, width);
  ```

**Puntuación**: 4/5 *(Falta profundizar en manejo de errores avanzados)*  

---

#### **2. ¿Qué fue lo más difícil?**  
- **Sincronización serial**: Entender por qué los datos se corrompían si no se usaba `\n` como delimitador.  
- **Detección de pulsaciones**: Implementar lógica para detectar el flanco de subida (`was_pressed` vs `is_pressed`).  

**Dificultad**: 4/5 *(Requiere comprensión de timing y buffers)*  

---

#### **3. ¿Qué fue lo más fácil?**  
- **Integración visual en p5.js**: Dibujar círculos y actualizar su posición con los datos del micro:bit.  
  ```javascript
  ellipse(microBitX, microBitY, 50);
  ```
**Facilidad**: 5/5 *(Similar a otros proyectos previos con p5.js)*  

---

#### **4. Tiempo dedicado**  
- **Total**: 5 sesiones (3 en clase, 2 fuera).  
- **¿Fue suficiente?**: 3/5 *(Necesité tiempo extra para depurar la comunicación serial)*  

---

#### **5. ¿Qué mejorarías?**  
- **Pruebas incrementales**: Validar cada función (ej: conexión, parsing) por separado antes de integrarlas.  
- **Documentar errores**: Registrar soluciones a problemas comunes (ej: "No se están recibiendo 4 datos").  

**Mejora potencial**: +1.5 puntos en eficiencia.  

---
### **Revisión de Aplicación Profesional para Ingeniería de Entretenimiento Digital**  

#### **6. Aplicación en Entretenimiento Digital**  
**Como futuro ingeniero en entretenimiento digital**, lo aprendido en esta unidad se aplica directamente a:  

1. **Desarrollo de Interfaces Físicas para Juegos**:  
   - Creación de controles personalizados usando micro:bit (ej: mandos con gestos o botones físicos para juegos indie).  
   ```python
   # Ejemplo: Detección de gestos para controlar personajes
   if accelerometer.current_gesture() == "shake":
       uart.write("JUMP\n")
   ```

2. **Experiencias Interactivas Inmersivas**:  
   - Integración de sensores físicos (acelerómetro, botones) con entornos virtuales en p5.js/Three.js para instalaciones artísticas.  
   ```javascript
   // En p5.js: Mover objetos 3D con datos del micro:bit
   if (microBitAState) avatar.jump(); 
   ```

3. **Prototipado Rápido de Dispositivos**:  
   - Validación de mecánicas interactivas para parques temáticos o escape rooms (ej: puzzles que requieren inclinar un dispositivo).  

4. **Educación Tecnológica**:  
   - Diseño de talleres sobre interacción hardware/software para niños o adolescentes.  

**Relevancia**: 5/5  
*(Estas habilidades son clave para innovar en gaming, realidad aumentada y experiencias interactivas multisensoriales)*.  

---

#### **Ejemplo Concreto: Juego con Controles Físicos**  
**Caso de Uso**: Un juego de plataformas donde:  
- **Inclinar el micro:bit** mueve al personaje.  
- **Botón A** salta.  
- **Botón B** activa habilidades.  

**Código micro:bit**:  
```python
while True:
    x = accelerometer.get_x()
    a = button_a.was_pressed()
    b = button_b.was_pressed()
    uart.write(f"{x},{a},{b}\n")
    sleep(50)
```

**Código p5.js**:  
```javascript
function readMicrobitData() {
    let data = port.readUntil("\n");
    let [x, a, b] = data.split(",");
    
    personaje.velocityX = map(x, -1024, 1024, -5, 5);
    if (a === "true") personaje.jump();
    if (b === "true") personaje.usePower();
}
```

---

¿Te gustaría que desarrolle otro ejemplo aplicado a tu campo? Por ejemplo: controlar efectos visuales en vivo con sensores o crear instrumentos musicales digitales.

---

#### **7. ¿Qué te gustaría aprender después?**  
- **Comunicación inalámbrica** (radio/Wi-Fi) entre micro:bit y p5.js.  
- **Procesamiento de señales**: Filtrar datos del acelerómetro para movimientos más suaves.  

---

#### **8. Estado de ánimo**  
- **Inicio**: 3/5 *(Frustración con errores de conexión)*  
- **Final**: 4.5/5 *(Satisfacción al ver el círculo moverse con el micro:bit)*  

---

#### **9. Motivación**  
- **Mantenimiento**: 4/5 *(La curiosidad por hacer funcionar el sistema me impulsó)*  
- **Dificultades**: Bajó al ver errores seriales, pero se recuperó con la solución.  

---

#### **10. Satisfacción con desempeño**  
- **Resultado final**: 4/5 *(Logré los objetivos, pero el código podría ser más modular)*  
- **Autoexigencia**: Quise implementar más efectos visuales (ej: trail de partículas).  

---

### **Promedio final**: 4.1/5  
**Reflexión**: Esta unidad consolidó mi capacidad para integrar hardware y software, aunque debo mejorar en documentación y pruebas tempranas. Estoy motivado para proyectos más complejos con comunicación inalámbrica.  
