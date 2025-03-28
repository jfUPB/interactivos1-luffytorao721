### **Actividad 08: Control Remoto desde p5.js y micro:bit**  

#### **🔹 Código para micro:bit (Micropython)**  
```python
from microbit import *
import utime
import music

# --- Configuración Inicial ---
uart.init(baudrate=115200)  # Comunicación serial

# Estados
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLOSION = 2

# Variables globales
current_state = STATE_CONFIG
start_time = 0
countdown_time = 20
sequence = []
correct_sequence = ['A', 'B', 'A', 'S']

# Sistema de eventos
event_occurred = False
current_event = None

def handle_events():
    global event_occurred, current_event
    
    # Eventos físicos
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
    
    # Eventos seriales (desde p5.js)
    if uart.any():
        incoming = uart.read(1).decode('utf-8').upper()
        if incoming in ['A', 'B', 'S', 'T']:
            current_event = incoming
            event_occurred = True

def update_bomb():
    global current_state, start_time, sequence, event_occurred
    
    if event_occurred:
        process_event(current_event)
        event_occurred = False
    
    if current_state == STATE_CONFIG:
        handle_config()
    elif current_state == STATE_ARMED:
        handle_armed()
    elif current_state == STATE_EXPLOSION:
        handle_explosion()

def process_event(event):
    global sequence
    if current_state == STATE_ARMED:
        sequence.append(event)
        if sequence[-4:] == correct_sequence:
            display.show(Image.YES)
            sleep(1000)
            reset_bomb()

def handle_config():
    global current_state, start_time, countdown_time
    display.show(str(countdown_time))
    
    if event_occurred and current_event == 'S':
        current_state = STATE_ARMED
        start_time = utime.ticks_ms()
        display.scroll("ARMADA!")
        sequence = []

def handle_armed():
    global current_state
    remaining = countdown_time - (utime.ticks_diff(utime.ticks_ms(), start_time) // 1000)
    
    if remaining <= 0:
        current_state = STATE_EXPLOSION
    else:
        display.show(str(remaining) if remaining > 5 or utime.ticks_ms() % 1000 < 500 else "")

def handle_explosion():
    global current_state
    music.play(music.POWER_UP)
    for _ in range(3):
        display.show(Image.SKULL)
        sleep(500)
        display.clear()
        sleep(200)
    reset_bomb()

def reset_bomb():
    global current_state, countdown_time
    current_state = STATE_CONFIG
    countdown_time = 20
    sequence = []

# --- Bucle principal ---
while True:
    handle_events()
    update_bomb()
    sleep(100)
```

---

### **🔹 Código para p5.js (Control Remoto)**  
```javascript
let port;
let connectBtn;

function setup() {
  createCanvas(300, 200);
  
  // Botones de control
  createButton('A').position(50, 50).mousePressed(() => sendCommand('A'));
  createButton('B').position(100, 50).mousePressed(() => sendCommand('B'));
  createButton('Shake (S)').position(150, 50).mousePressed(() => sendCommand('S'));
  createButton('Touch (T)').position(220, 50).mousePressed(() => sendCommand('T'));
  
  // Conexión serial
  port = createSerial();
  connectBtn = createButton('Conectar a micro:bit');
  connectBtn.position(80, 120);
  connectBtn.mousePressed(toggleConnection);
}

function draw() {
  background(240);
  text("Control de Bomba", 100, 30);
  text("Estado: " + (port.opened() ? "Conectado" : "Desconectado"), 100, 80);
}

function toggleConnection() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
    connectBtn.html('Desconectar');
  } else {
    port.close();
    connectBtn.html('Conectar');
  }
}

function sendCommand(cmd) {
  if (port.opened()) {
    port.write(cmd);
    console.log("Comando enviado:", cmd);
  } else {
    alert("¡Conecta primero el micro:bit!");
  }
}
```

---
