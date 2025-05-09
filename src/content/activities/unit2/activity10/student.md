### **Actividad 10: Controlando la Pantalla con una Máquina de Estados y Concurrencia**

En esta actividad, analizaremos un programa que utiliza una **máquina de estados** para gestionar la concurrencia entre una secuencia de imágenes predefinida y la respuesta a la pulsación de botones. El programa muestra diferentes expresiones en la pantalla del micro:bit según un ciclo de tiempo, pero también reacciona inmediatamente si se presiona el botón **A**.

---

### **Descripción del Problema**

El programa tiene los siguientes comportamientos:
1. **Ciclo de Expresiones**:
   - Muestra una **cara feliz** durante **1.5 segundos**.
   - Cambia a una **expresión sonriente** durante **1 segundo**.
   - Muestra una **cara triste** durante **2 segundos**.
   - El ciclo se repite indefinidamente.

2. **Interacción con el Botón A**:
   - Si se presiona el botón **A** mientras se muestra la **cara feliz** o la **expresión sonriente**, el micro:bit interrumpe el ciclo y muestra inmediatamente la **cara triste** o **feliz**, respectivamente.
   - Si se presiona el botón **A** mientras se muestra la **cara triste**, el micro:bit cambia a la **expresión sonriente**.

---

### **Código en Micropython**

```python
from microbit import *
import utime

# Definición de estados
STATE_INIT = 0
STATE_HAPPY = 1
STATE_SMILE = 2
STATE_SAD = 3

# Intervalos de tiempo para cada estado
HAPPY_INTERVAL = 1500  # 1.5 segundos
SMILE_INTERVAL = 1000  # 1 segundo
SAD_INTERVAL = 2000    # 2 segundos

# Variables de estado
current_state = STATE_INIT
start_time = 0
interval = 0

while True:
    # Estado inicial
    if current_state == STATE_INIT:
        display.show(Image.HAPPY)  # Muestra cara feliz
        start_time = utime.ticks_ms()  # Guarda el tiempo inicial
        interval = HAPPY_INTERVAL  # Define el intervalo para el estado HAPPY
        current_state = STATE_HAPPY  # Cambia al estado HAPPY

    # Estado HAPPY
    elif current_state == STATE_HAPPY:
        if button_a.was_pressed():  # Si se presiona el botón A
            display.show(Image.SAD)  # Muestra cara triste
            start_time = utime.ticks_ms()  # Reinicia el tiempo
            interval = SAD_INTERVAL  # Define el intervalo para el estado SAD
            current_state = STATE_SAD  # Cambia al estado SAD
        elif utime.ticks_diff(utime.ticks_ms(), start_time) > interval:  # Si pasa el tiempo
            display.show(Image.SMILE)  # Muestra expresión sonriente
            start_time = utime.ticks_ms()  # Reinicia el tiempo
            interval = SMILE_INTERVAL  # Define el intervalo para el estado SMILE
            current_state = STATE_SMILE  # Cambia al estado SMILE

    # Estado SMILE
    elif current_state == STATE_SMILE:
        if button_a.was_pressed():  # Si se presiona el botón A
            display.show(Image.HAPPY)  # Muestra cara feliz
            start_time = utime.ticks_ms()  # Reinicia el tiempo
            interval = HAPPY_INTERVAL  # Define el intervalo para el estado HAPPY
            current_state = STATE_HAPPY  # Cambia al estado HAPPY
        elif utime.ticks_diff(utime.ticks_ms(), start_time) > interval:  # Si pasa el tiempo
            display.show(Image.SAD)  # Muestra cara triste
            start_time = utime.ticks_ms()  # Reinicia el tiempo
            interval = SAD_INTERVAL  # Define el intervalo para el estado SAD
            current_state = STATE_SAD  # Cambia al estado SAD

    # Estado SAD
    elif current_state == STATE_SAD:
        if button_a.was_pressed():  # Si se presiona el botón A
            display.show(Image.SMILE)  # Muestra expresión sonriente
            start_time = utime.ticks_ms()  # Reinicia el tiempo
            interval = SMILE_INTERVAL  # Define el intervalo para el estado SMILE
            current_state = STATE_SMILE  # Cambia al estado SMILE
        elif utime.ticks_diff(utime.ticks_ms(), start_time) > interval:  # Si pasa el tiempo
            display.show(Image.HAPPY)  # Muestra cara feliz
            start_time = utime.ticks_ms()  # Reinicia el tiempo
            interval = HAPPY_INTERVAL  # Define el intervalo para el estado HAPPY
            current_state = STATE_HAPPY  # Cambia al estado HAPPY
```

---

### **Análisis del Código**

#### **1. Estructura de la Máquina de Estados**
El programa utiliza una **máquina de estados** para gestionar el comportamiento del micro:bit. Los estados son:
- **STATE_INIT**: Estado inicial que configura el primer estado (`STATE_HAPPY`).
- **STATE_HAPPY**: Muestra una cara feliz y cambia a `STATE_SMILE` después de 1.5 segundos o a `STATE_SAD` si se presiona el botón **A**.
- **STATE_SMILE**: Muestra una expresión sonriente y cambia a `STATE_SAD` después de 1 segundo o a `STATE_HAPPY` si se presiona el botón **A**.
- **STATE_SAD**: Muestra una cara triste y cambia a `STATE_HAPPY` después de 2 segundos o a `STATE_SMILE` si se presiona el botón **A**.

#### **2. Eventos y Transiciones**
- **Evento de Tiempo**: El programa verifica si ha transcurrido el tiempo especificado para cada estado usando `utime.ticks_diff()`.
- **Evento de Botón**: El programa verifica si se ha presionado el botón **A** usando `button_a.was_pressed()`.

#### **3. Concurrencia**
El programa maneja la concurrencia entre la secuencia de imágenes y la respuesta al botón **A** mediante la máquina de estados. Esto permite que el micro:bit reaccione inmediatamente a la interacción del usuario sin interrumpir el flujo principal del programa.

---

### **Prueba del Programa**

1. **Carga el Código en el micro:bit**:
   - Conecta el micro:bit a la computadora y carga el código usando el editor de Micropython.

2. **Observa el Comportamiento**:
   - El micro:bit mostrará las expresiones en el siguiente orden:
     - Cara feliz (1.5 segundos) → Expresión sonriente (1 segundo) → Cara triste (2 segundos) → Cara feliz (1.5 segundos) → ...
   - Presiona el botón **A** en diferentes momentos para ver cómo interrumpe el ciclo y cambia las expresiones.

---

### **Resultados Esperados**

- **Ciclo Normal**:
  - Cara feliz → Expresión sonriente → Cara triste → Cara feliz → ...

- **Interacción con el Botón A**:
  - Si se presiona el botón **A** durante la **cara feliz** o la **expresión sonriente**, el micro:bit muestra la **cara triste**.
  - Si se presiona el botón **A** durante la **cara triste**, el micro:bit muestra la **expresión sonriente**.



