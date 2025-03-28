## **Actividad 10: Consolidación y Metacognición**  


### **1. Ventajas de las Máquinas de Estado en Escalabilidad**  
La técnica de **máquinas de estados finitos (FSM)** es poderosa para aplicaciones interactivas como la bomba porque:  

#### **✔ Ventajas en Concurrencia y Manejo de Eventos**  
- **Claridad en flujos lógicos**: Cada estado (`CONFIGURATION`, `ARMED`, `EXPLOSION`) encapsula su comportamiento, evitando código espagueti.  
- **Manejo seguro de eventos**: Las transiciones (ej: `A → ARMED`) son explícitas, lo que reduce errores cuando múltiples eventos ocurren (ej: teclado + micro:bit).  
- **Escalabilidad**: Añadir nuevos estados (ej: `PAUSED`) es sencillo sin romper la lógica existente.  

#### **✘ Desventajas**  
- **Puede volverse complejo** si hay demasiados estados (solvable con sub-estados o jerarquías).  
- **Requiere planificación inicial** para definir transiciones válidas.  

---

### **2. Evaluación de las Pruebas Realizadas**  

#### **✔ Ventajas de las Pruebas**  
- **Cobertura de casos críticos**: Se probaron todas las transiciones de estados y eventos (teclado, micro:bit, temporizador).  
- **Detección temprana de errores**: Ej: La prueba **P7** reveló que la secuencia no se reiniciaba al armar la bomba.  
- **Documentación implícita**: Las pruebas sirven como especificación del comportamiento esperado.  

#### **✘ Desventajas**  
- **Dependencia de condiciones ideales**: Algunas pruebas (ej: conexión micro:bit) requieren hardware físico.  
- **No automatizadas**: Se ejecutaron manualmente (podrían mejorarse con frameworks como Jest).  

#### **Importancia de las Pruebas de Regresión**  
- **Garantizan que nuevas modificaciones no rompan funcionalidades existentes**.  
  - Ej: Si añades un botón "PAUSA" en `ARMED`, debes verificar que no afecte la secuencia de desactivación.  
- **Evitan "errores silenciosos"**: Un cambio aparentemente inocuo (ej: ajuste de temporizador) podría invalidar pruebas previas.  

**Consecuencias de no hacerlas**:  
- **Errores en producción**: La bomba podría explotar incluso con la secuencia correcta si una modificación altera el flujo.  
- **Costo alto de reparación**: Detectar un bug tarde requiere más tiempo que corregirlo durante el desarrollo.  

---

### **3. Metacognición: Ajustes para Continuar el Curso**  

#### **¿Qué funcionó bien?**  
- **FSM fue clave** para manejar la complejidad de estados y eventos.  
- **Pruebas manuales exhaustivas** cubrieron escenarios críticos.  

#### **¿Qué mejorar?**  
- **Automatizar pruebas** (ej: con p5.js + Jest).  
- **Documentar mejor los edge cases** (ej: ¿Qué pasa si el micro:bit se desconecta?).  
- **Refactorizar código** para hacerlo más modular (ej: separar lógica de UI).  

#### **Plan de Acción**  
1. **Automatizar pruebas** en la próxima actividad.  
2. **Añadir más validaciones** (ej: timeout si el micro:bit no responde).  
3. **Explorar FSM jerárquicas** si la complejidad aumenta.  

