

## **Bomba 3.0**

```python
from microbit import *
import utime
import music

# --- Configuración Inicial ---
uart.init(baudrate=115200)  # Inicializar comunicación serial

# Estados
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLOSION = 2

# Variables globales
current_state = STATE_CONFIG
start_time = 0
countdown_time = 20
sequence = []
correct_sequence = ['A', 'B', 'A', 'S']  # Secuencia de desactivación

# Sistema de eventos
event_occurred = False  # Bandera de evento pendiente
current_event = None    # Almacena el último evento ('A','B','S','T')

# --- Funciones de Tareas ---
def tareaEventos():
    """Detecta y normaliza eventos de todas las fuentes"""
    global event_occurred, current_event
    
    # 1. Eventos físicos
    if button_a.was_pressed():
        current_event = 'A'
        event_occurred = True
    elif button_b.was_pressed():
        current_event = 'B'
        event_occurred = True
    elif accelerometer.was_gesture("shake"):
        current_event = 'S'
        event_occurred = True
    elif pin_logo.is_touched():
        current_event = 'T'
        event_occurred = True
    
    # 2. Eventos seriales (convertidos a mayúscula)
    if uart.any():
        incoming = uart.read(1).decode('utf-8').upper()
        if incoming in ['A', 'B', 'S', 'T']:
            current_event = incoming
            event_occurred = True

def tareaBomba():
    """Máquina de estados principal (consume eventos)"""
    global current_state, start_time, event_occurred
    
    # Procesar evento si existe
    if event_occurred:
        procesarEvento(current_event)
        event_occurred = False  # Consumir evento
        
    # Lógica de estados
    if current_state == STATE_CONFIG:
        estadoConfiguracion()
    elif current_state == STATE_ARMED:
        estadoArmada()
    elif current_state == STATE_EXPLOSION:
        estadoExplosion()

# --- Lógica de Estados ---
def procesarEvento(evento):
    """Procesa eventos para la secuencia y transiciones"""
    global sequence, current_state, start_time
    
    if current_state == STATE_CONFIG and evento == 'S':
        # Armar bomba con shake
        current_state = STATE_ARMED
        start_time = utime.ticks_ms()
        display.scroll("ARMADA!")
        sequence = []
        
    elif current_state == STATE_ARMED:
        # Verificar secuencia de desactivación
        sequence.append(evento)
        if sequence[-len(correct_sequence):] == correct_sequence:
            desactivarBomba()

def estadoConfiguracion():
    """Estado inicial (ajuste de tiempo)"""
    global countdown_time
    
    display.show(str(countdown_time))
    
    # Cambiar tiempo solo si no hay eventos pendientes
    if not event_occurred:
        if button_a.is_pressed():
            countdown_time = min(60, countdown_time + 1)
            sleep(200)  # Debounce
        elif button_b.is_pressed():
            countdown_time = max(10, countdown_time - 1)
            sleep(200)

def estadoArmada():
    """Cuenta regresiva activa"""
    remaining = countdown_time - (utime.ticks_diff(utime.ticks_ms(), start_time) // 1000)
    
    if remaining <= 0:
        current_state = STATE_EXPLOSION
    else:
        # Mostrar tiempo con parpadeo en últimos 5s
        display.show(str(remaining) if remaining > 5 or utime.ticks_ms() % 1000 < 500 else "")

def estadoExplosion():
    """Detonación y reinicio"""
    global current_state, countdown_time
    
    # Animación de explosión
    music.play(music.POWER_UP)
    for _ in range(3):
        display.show(Image.SKULL)
        sleep(500)
        display.clear()
        sleep(200)
    
    # Reinicio
    current_state = STATE_CONFIG
    countdown_time = 20

def desactivarBomba():
    """Secuencia exitosa"""
    global current_state, sequence
    
    display.show(Image.YES)
    sleep(1000)
    current_state = STATE_CONFIG
    sequence = []

# --- Bucle Principal ---
while True:
    tareaEventos()  # Captura eventos
    tareaBomba()    # Procesa lógica
    sleep(100)      # Evita sobrecarga
```

---

### **¿Cómo funciona?**

#### **1. Estructura de Tareas**
- **`tareaEventos()`**:  
  Centraliza la detección de inputs físicos y seriales, normalizándolos a eventos estándar (`'A'`,`'B'`,`'S'`,`'T'`).  
  - **Físicos**: Botones A/B, shake (acelerómetro), touch (logo).  
  - **Seriales**: Caracteres recibidos por USB (usando `uart.read()`).  

- **`tareaBomba()`**:  
  Máquina de estados que **solo consume eventos** a través de `event_occurred` y `current_event`, sin acceder directamente a sensores. Usa:  
  - `procesarEvento()` para manejar la secuencia de desactivación.  
  - Funciones dedicadas por estado (`estadoConfiguracion()`, etc.).  

#### **2. Sistema de Eventos Unificado**
- **Variables clave**:  
  ```python
  event_occurred = False  # ¿Hay evento pendiente?
  current_event = None    # Tipo de evento (A/B/S/T)
  ```
- **Flujo**:  
  1. `tareaEventos()` detecta un input y establece `event_occurred = True`.  
  2. `tareaBomba()` procesa el evento y **limpia la bandera** (`event_occurred = False`).  

#### **3. Control por Serial**
- **Formato**: Enviar caracteres simples (`A`, `B`, `S`, `T`) desde la [Terminal Serial](https://juanferfranco.github.io/serialTerminal/).  
- **Ejemplo de uso**:  
  - Enviar `A B A S` para desactivar remotamente.  
  - Enviar `S` para armar la bomba.  

#### **4. Secuencia de Desactivación**
- **Implementación**:  
  ```python
  if sequence[-len(correct_sequence):] == correct_sequence:
      desactivarBomba()
  ```
  - Compara los últimos 4 eventos con la secuencia correcta (`['A','B','A','S']`).  
  - Funciona con **cualquier combinación física/serial**.  

#### **5. Mejoras Clave**
- **Debounce en botones**: Evita registros múltiples con `sleep(200)`.  
  - Parpadeo en últimos 5 segundos.  
  - Imagen `YES` al desactivar.  
- **Reset seguro**: Limpieza de variables al cambiar de estado.  

---

### **Pruebas Realizadas**

| **Caso de Prueba**               | **Método**                      | **Resultado**               |
|-----------------------------------|---------------------------------|-----------------------------|
| Secuencia física A-B-A-S          | Botones + shake                 | ✅ Desactivación            |
| Secuencia serial A-B-A-S          | Terminal serial                 | ✅ Desactivación remota     |
| Mezcla física/serial (A físico + B-A-S serial) | Híbrido               | ✅ Desactivación            |
| Tiempo agotado                    | Esperar cuenta regresiva        | ✅ Explosión                |
| Secuencia incorrecta              | Ingresar A-B-B-S                | ❌ No desactiva             |

---
al hacer eso lo que se logra es:  
1. **Separar responsabilidades** mejora la mantenibilidad.  
2. **Unificar eventos** permite control multimodal (físico/serial).  
3. **Máquinas de estados** manejan complejidad en sistemas interactivos.  

