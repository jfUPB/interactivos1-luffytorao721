### **Vectores de Prueba para la Aplicación de la Bomba (p5.js + micro:bit)**

Para garantizar que la aplicación funcione correctamente, definimos vectores de prueba que cubran todos los estados, eventos y posibles fallos. Consideramos tanto **eventos generados en p5.js** (teclado) como **eventos del micro:bit** (botones A/B, agitado, acelerómetro).

---

## **1. Pruebas de Estados y Transiciones**
### **Estado: CONFIGURACIÓN**
| **Prueba** | **Entrada** | **Resultado Esperado** | **Resultado Obtenido** |
|------------|-------------|------------------------|------------------------|
| **P1** | Presionar `A` (tecla) | Aumenta tiempo en 1s (hasta 60s) | ✅ |
| **P2** | Presionar `B` (tecla) | Disminuye tiempo en 1s (hasta 10s) | ✅ |
| **P3** | Presionar `S` (tecla) | Cambia a estado **ARMED** | ✅ |
| **P4** | Agitar micro:bit (gesto) | Cambia a estado **ARMED** | ✅ (si está conectado) |
| **P5** | Intentar ajustar tiempo fuera de límites (ej: 9s o 61s) | Mantiene 10s (mín) o 60s (máx) | ✅ |

### **Estado: ARMADO (Cuenta Regresiva)**
| **Prueba** | **Entrada** | **Resultado Esperado** | **Resultado Obtenido** |
|------------|-------------|------------------------|------------------------|
| **P6** | Temporizador llega a **0** | Explota (estado **EXPLOSION**) | ✅ |
| **P7** | Ingresar secuencia **A-B-A-S** (teclas) | Bomba **se desactiva** (vuelve a CONFIG) | ✅ |
| **P8** | Ingresar secuencia **incorrecta** (ej: A-B-B-S) | **No** desactiva la bomba | ✅ |
| **P9** | Últimos **5 segundos** | Texto parpadea en rojo | ✅ |
| **P10** | Botón **A** en micro:bit | Añade "A" a la secuencia | ✅ (si está conectado) |
| **P11** | Botón **B** en micro:bit | Añade "B" a la secuencia | ✅ (si está conectado) |

### **Estado: EXPLOSIÓN**
| **Prueba** | **Entrada** | **Resultado Esperado** | **Resultado Obtenido** |
|------------|-------------|------------------------|------------------------|
| **P12** | **2 segundos** después de explotar | Reinicia a **CONFIGURACIÓN** | ✅ |

---

## **2. Pruebas de Regresión**
Si alguna prueba falla, se debe:
1. **Identificar el error** (ej: secuencia no se reinicia).
2. **Corregir el código** (ej: resetear `sequence = []` al armar la bomba).
3. **Volver a ejecutar todas las pruebas** desde el principio.

### **Ejemplo de Prueba Fallida y Corrección**
| **Prueba Fallida** | **Causa** | **Solución** |
|--------------------|-----------|--------------|
| **P7** (Secuencia correcta no desactiva) | `sequence` no se reiniciaba al armar | Añadir `sequence = []` en `armBomb()` |  

---

## **3. Fuentes de Eventos Probadas**
| **Fuente** | **Evento** | **Estado Aplicable** |
|------------|------------|----------------------|
| **Teclado (p5.js)** | `A`, `B`, `S` | **CONFIG**, **ARMED** |
| **micro:bit** | Botón **A** | **ARMED** |
| **micro:bit** | Botón **B** | **ARMED** |
| **micro:bit** | **Agitado** (gesto) | **CONFIG** → **ARMED** |
| **Temporizador** | **Cuenta regresiva** | **ARMED** → **EXPLOSION** |

---

## **4. Casos No Modelados (Pruebas Adicionales)**
| **Prueba** | **Descripción** | **Resultado Esperado** |
|------------|-----------------|------------------------|
| **P13** | Desconexión del micro:bit durante **ARMED** | La bomba sigue contando en p5.js |
| **P14** | Reinicio manual (F5) durante **ARMED** | La bomba se reinicia |
| **P15** | Entrada simultánea (teclado + micro:bit) | Ambas entradas se registran |

---

