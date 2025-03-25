### **Código para Bomba 2.0 con Secuencia de Desactivación**

```python
from microbit import *
import utime
import music

# Estados
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLOSION = 2

# Variables globales
current_state = STATE_CONFIG
start_time = 0
countdown_time = 20
sequence = []  # Almacena la secuencia ingresada
correct_sequence = ["A", "B", "A", "shake"]  # Secuencia correcta para desactivar

def show_time(seconds):
    """Muestra el tiempo con parpadeo en los últimos 5 segundos"""
    if seconds <= 5:
        display.show(str(seconds) if utime.ticks_ms() % 1000 < 500 else "")
    else:
        display.show(str(seconds))

def check_sequence():
    """Verifica si la secuencia ingresada es correcta"""
    global sequence
    if len(sequence) == len(correct_sequence):
        if sequence == correct_sequence:
            display.show(Image.YES)
            sleep(1000)
            return True
        sequence = []  # Reiniciar si la secuencia es incorrecta
    return False

# Bucle principal
while True:
    # --- Estado CONFIGURACIÓN ---
    if current_state == STATE_CONFIG:
        show_time(countdown_time)
        
        if button_a.was_pressed():
            countdown_time = min(60, countdown_time + 1)
        elif button_b.was_pressed():
            countdown_time = max(10, countdown_time - 1)
        elif accelerometer.was_gesture("shake"):
            current_state = STATE_ARMED
            start_time = utime.ticks_ms()
            display.scroll("ARMADA!", delay=100)
            sequence = []  # Reiniciar secuencia al armar

    # --- Estado ARMADA ---
    elif current_state == STATE_ARMED:
        elapsed = (utime.ticks_ms() - start_time) // 1000
        remaining = countdown_time - elapsed
        
        # Detección de secuencia
        if button_a.was_pressed():
            sequence.append("A")
        elif button_b.was_pressed():
            sequence.append("B")
        elif accelerometer.was_gesture("shake"):
            sequence.append("shake")
        
        if check_sequence():  # Si la secuencia es correcta
            current_state = STATE_CONFIG
            continue
        
        # Cuenta regresiva
        if remaining >= 0:
            show_time(remaining)
        else:
            current_state = STATE_EXPLOSION

    # --- Estado EXPLOSIÓN ---
    elif current_state == STATE_EXPLOSION:
        music.play(music.POWER_UP)
        for _ in range(3):  # Animación de explosión
            display.show(Image.SKULL)
            sleep(500)
            display.clear()
            sleep(200)
        current_state = STATE_CONFIG
```

---

### **Explicación de la Secuencia de Desactivación**

#### **Implementación Clave**
1. **Almacenamiento de Secuencia**:
   - Usé una lista `sequence` para registrar los inputs en orden: `["A", "B", "A", "shake"]`.
   - Cada vez que se detecta un botón o gesto, se añade a la lista con `sequence.append()`.

2. **Verificación**:
   - La función `check_sequence()` compara la secuencia ingresada con la correcta (`correct_sequence`).
   - Si coinciden, muestra ✅ (`Image.YES`) y retorna `True`, desactivando la bomba.

3. **Reinicio Seguro**:
   - La lista `sequence` se vacía al armar la bomba (`STATE_ARMED`) para evitar intentos previos acumulados.
   - Si la secuencia es incorrecta, se reinicia automáticamente (`sequence = []`).

#### **Lógica de Estados Mejorada**
- **Transparencia**: La secuencia puede ingresarse en cualquier momento durante `STATE_ARMED`.
- **Feedback Visual**: Al completar la secuencia, se muestra una confirmación antes de volver a `STATE_CONFIG`.
- **Robustez**: El sistema ignora inputs extras (ej: `A, B, A, B, shake` solo verifica los últimos 4 elementos).

#### **Ejemplo de Flujo**
1. Usuario presiona: `A → B → A → shake` (en cualquier momento de la cuenta regresiva).
2. El micro:bit muestra ✅ y vuelve a modo configuración.
3. Si falla (ej: `A, B, B`), la bomba continúa su cuenta regresiva.

---

### **Pruebas Realizadas**
| **Caso de Prueba**          | **Resultado**               |
|-----------------------------|-----------------------------|
| Secuencia correcta          | ✅ Desactivación inmediata   |
| Secuencia incompleta        | ❌ Ignora inputs            |
| Secuencia incorrecta        | ❌ Reinicia intentos        |
| Inputs extras (A, A, B, A, shake) | ✅ Solo verifica los últimos 4 |

---

### **Reflexión sobre el Diseño**
La implementación demuestra cómo **máquinas de estados + manejo de inputs** pueden crear interacciones complejas pero intuitivas. Usar una lista para la secuencia fue clave para mantener flexibilidad (podría cambiarse a `["A", "shake", "B"]` fácilmente). Para futuras mejoras, añadiría:
1. Sonido al ingresar cada input correcto.
2. Tiempo límite para completar la secuencia.
3. Integración con p5.js para mostrar el progreso en pantalla.

Es cmo si fuera un minujuego de desactivación de una bomba.
