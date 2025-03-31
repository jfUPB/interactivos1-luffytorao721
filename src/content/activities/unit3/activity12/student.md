### **Autoevaluación**  

#### **Conceptos que Aprendí**  
1. **Máquinas de Estado (FSM)**  
   - **Ejemplo**: Implementé correctamente los estados `CONFIGURATION`, `ARMED` y `EXPLOSION` en el simulador de bomba, definiendo transiciones claras entre ellos (como pasar a `ARMED` al presionar 'S' o agitar el micro:bit, y a `EXPLOSION` cuando el temporizador llega a 0).  
   - **¿Dónde está la evidencia?**: El código responde adecuadamente a cada evento y mantiene consistencia en los estados, sin mezclar comportamientos.  

2. **Manejo de Eventos Concurrentes**  
   - **Ejemplo**: Logré que la bomba procesara inputs simultáneos del teclado (p5.js) y del micro:bit (botones A/B, gesto de agitado) sin conflictos.  
   - **¿Dónde está la evidencia?**: Al presionar 'A' en el teclado y el botón B del micro:bit casi al mismo tiempo, ambos eventos se registran correctamente en la secuencia de desactivación.  

3. **Integración Hardware-Software**  
   - **Ejemplo**: Conecté el micro:bit mediante comunicación serial para controlar la bomba desde el dispositivo físico.  
   - **¿Dónde está la evidencia?**: El sketch en p5.js reacciona a los botones del micro:bit y al gesto de agitado, actualizando la interfaz en tiempo real.  

4. **Pruebas de Regresión**  
   - **Ejemplo**: Identifiqué y corregí un error donde la secuencia de desactivación no se reiniciaba al armar la bomba, verificando luego que el cambio no afectara otras funcionalidades.  
   - **¿Dónde está la evidencia?**: Tras la corrección, todas las pruebas previas (ajuste de tiempo, desactivación, explosión) siguieron funcionando.  

---

#### **Conceptos que no Aprendí Completamente**  
1. **Manejo de Errores en Conexión Serial**  
   - **Dificultad**: No implementé un sistema robusto para casos como la desconexión abrupta del micro:bit durante el estado `ARMED`.  
   - **Ejemplo**: Si el micro:bit se desconecta, la bomba en p5.js sigue contando, pero no hay feedback visual para el usuario sobre el fallo.  

2. **Optimización de Código para Legibilidad**  
   - **Dificultad**: Mi implementación de la FSM usa condicionales anidados (`if/else`), lo que dificulta añadir nuevos estados.  
   - **Ejemplo**: Para agregar un estado `PAUSED`, tendría que modificar múltiples partes del código, aumentando el riesgo de errores.  

3. **Pruebas Automatizadas**  
   - **Dificultad**: Las pruebas se ejecutaron manualmente; no usé frameworks como Jest para automatizarlas.  
   - **Ejemplo**: Validar la secuencia `A-B-A-S` requiere ingresarla manualmente cada vez que se hace un cambio en el código.  

---

#### **Estrategias para Mejorar**  

1. **Manejo de Errores en Serial**  
   - **Acción**: Investigaré el uso de `try/catch` y eventos `onDisconnect` en la API Web Serial.  
   - **Recursos**: Documentación de Web Serial API y ejemplos en GitHub.  
   - **Tiempo**: 2 horas semanales.  
   

2. **Refactorización con Enfoque en Escalabilidad**  
   - **Acción**: Reestructuraré la FSM usando un objeto con estados y transiciones definidas (ej: `{ state: "ARMED", transitions: [...] }`).  
   - **Recursos**: Libro "Clean Code" de Robert C. Martin y tutoriales sobre patrones de diseño.  
   - **Tiempo**: 1 hora diaria.  

3. **Automatización de Pruebas**  
   - **Acción**: Configuraré Jest para probar funciones clave (ej: `checkSequence()`) con casos predefinidos.  
   - **Recursos**: Guías de Jest para p5.js y proyectos open-source de testing.  
 

