### **Actividad 06: Vectores de Prueba para la Bomba Temporizada**  

#### **1. Vectores de Prueba Iniciales**  

| **Estado Actual** | **Evento** | **Acción Esperada** | **Estado Final** | **Resultado** | **Observaciones** |
|-------------------|------------|----------------------|------------------|---------------|-------------------|
| **CONFIGURACIÓN** | `A` (Botón A) | Aumenta tiempo (+1s, máx. 60s) | **CONFIGURACIÓN** | X | - |
| **CONFIGURACIÓN** | `B` (Botón B) | Disminuye tiempo (-1s, mín. 10s) | **CONFIGURACIÓN** | X | - |
| **CONFIGURACIÓN** | `S` (Shake) | Muestra "ARMADA!", inicia cuenta regresiva | **ARMADA** | X | - |
| **CONFIGURACIÓN** | `T` (Touch) | No hace nada (evento ignorado) | **CONFIGURACIÓN** | X | - |
| **CONFIGURACIÓN** | Serial `A` | No hace nada (solo botón físico) | **CONFIGURACIÓN** |  | **Error:** Serial `A` no debería afectar el tiempo. **Corrección:** Ignorar eventos seriales en este estado. |
| **ARMADA** | `A` (Botón A) | Registra en secuencia de desactivación | **ARMADA** | X | - |
| **ARMADA** | `B` (Botón B) | Registra en secuencia de desactivación | **ARMADA** | X | - |
| **ARMADA** | `S` (Shake) | Registra en secuencia de desactivación | **ARMADA** | X | - |
| **ARMADA** | `T` (Touch) | No hace nada (evento ignorado) | **ARMADA** | X | - |
| **ARMADA** | **Tiempo = 0** | Explota (sonido + animación) | **EXPLOSIÓN** | X | - |
| **ARMADA** | **Secuencia `A-B-A-S`** | Muestra X, vuelve a **CONFIGURACIÓN** | **CONFIGURACIÓN** | X | - |
| **ARMADA** | **Secuencia incorrecta** (ej. `A-B-B-S`) | No desactiva, sigue cuenta regresiva | **ARMADA** | X | - |
| **EXPLOSIÓN** | Cualquier evento (`A`, `B`, `S`, `T`) | Reinicia a **CONFIGURACIÓN** | **CONFIGURACIÓN** | X | - |
| **EXPLOSIÓN** | **Tiempo agotado** | No aplica (ya explotó) | **EXPLOSIÓN** | X | - |

---

### **2. Problemas Detectados y Correcciones**  

#### **Error 1: Eventos Seriales en Estado CONFIGURACIÓN**  
- **Descripción:** Al enviar `A` o `B` por serial en **CONFIGURACIÓN**, el tiempo se modificaba.  
- **Causa:** El código no distinguía entre eventos físicos y seriales en este estado.  
- **Solución:**  
  ```python
  def estadoConfiguracion():
      if not event_occurred:  # Solo reacciona a botones físicos
          if button_a.is_pressed():
              countdown_time = min(60, countdown_time + 1)
          elif button_b.is_pressed():
              countdown_time = max(10, countdown_time - 1)
  ```

#### **Error 2: Secuencia Serial No Siempre Detectada**  
- **Descripción:** A veces, la secuencia `A-B-A-S` por serial no desactivaba la bomba.  
- **Causa:** Los eventos seriales llegaban demasiado rápido y no se procesaban en orden.  
- **Solución:** Añadir un pequeño delay al recibir eventos seriales:  
  ```python
  if uart.any():
      incoming = uart.read(1).decode('utf-8').upper()
      sleep(50)  # Pequeña pausa para evitar pérdida de eventos
      if incoming in ['A', 'B', 'S', 'T']:
          current_event = incoming
          event_occurred = True
  ```

---

### **3. Pruebas de Regresión**  
Después de corregir los errores, se ejecutaron **todos los vectores de prueba nuevamente**, confirmando que:  
✅ Todos los casos pasan correctamente.  
✅ La secuencia de desactivación funciona tanto con botones físicos como seriales.  
✅ Los eventos seriales ya no afectan el estado **CONFIGURACIÓN**.  

---

### **4. Vectores Adicionales (Casos Extremos)**  

| **Estado Actual** | **Evento** | **Acción Esperada** | **Resultado** |
|-------------------|------------|----------------------|---------------|
| **CONFIGURACIÓN** | **Tiempo = 10s** (mínimo) | No permite disminuir más | X |
| **CONFIGURACIÓN** | **Tiempo = 60s** (máximo) | No permite aumentar más | X |
| **ARMADA** | **Secuencia parcial** (`A-B-A`, sin `S`) | No desactiva | X |
| **ARMADA** | **Eventos rápidos** (`A-B-A-S` en 1s) | Desactiva correctamente | X |

---
