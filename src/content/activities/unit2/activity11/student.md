### **Diseño de la Lógica de una Bomba Temporizada**

El sistema de la bomba temporizada está diseñado para ser utilizado en un escape room. Su funcionamiento se basa en una **máquina de estados** que gestiona los diferentes modos de operación, la interacción con los sensores y la respuesta de los actuadores. A continuación, se describe el comportamiento del sistema en cada estado y cómo responde a los eventos.

---

#### **1. Modo de Configuración**
En el **modo de configuración**, la bomba está **desarmada** y no realiza ninguna cuenta regresiva. El tiempo inicial se establece en **20 segundos**, pero el usuario puede ajustarlo utilizando los botones **A** (UP) y **B** (DOWN). Cada vez que se presiona el botón **A**, el tiempo aumenta en **1 segundo**, y si se presiona el botón **B**, disminuye en **1 segundo**. El tiempo siempre está limitado entre **10 y 60 segundos**, y se muestra en la pantalla de LEDs.

- **Detalles Técnicos**:
  - El tiempo se almacena en una variable (`countdown_time`).
  - Los botones **A** y **B** se verifican en cada iteración del bucle principal.
  - El tiempo actual se muestra en la pantalla de LEDs usando `display.show()`.

---

#### **2. Armado de la Bomba**
Cuando el usuario realiza el gesto de **shake** (agitación), la bomba cambia al estado **ARMADA**. En este estado, comienza la **cuenta regresiva**, y el tiempo restante se muestra en la pantalla de LEDs. El sistema utiliza la función `utime.ticks_ms()` para medir el tiempo transcurrido y actualizar el tiempo restante.

- **Detalles Técnicos**:
  - El gesto de shake se detecta usando `accelerometer.was_gesture("shake")`.
  - El tiempo inicial se guarda en una variable (`start_time`).
  - El tiempo restante se calcula restando el tiempo transcurrido del tiempo inicial.

---

#### **3. Cuenta Regresiva**
Durante la **cuenta regresiva**, el sistema verifica constantemente si el tiempo ha llegado a **0**. Si es así, la bomba cambia al estado **EXPLOSIÓN**. Además, el usuario puede desactivar la bomba en cualquier momento tocando el botón **touch**, lo que hace que el sistema vuelva al **modo de configuración**.

- **Detalles Técnicos**:
  - El tiempo restante se muestra en la pantalla de LEDs usando `display.show()`.
  - El botón **touch** se verifica en cada iteración del bucle principal.
  - Si el tiempo llega a 0, el sistema cambia al estado **EXPLOSIÓN**.

---

#### **4. Explosión**
Cuando el tiempo llega a **0**, la bomba cambia al estado **EXPLOSIÓN**. En este estado, el sistema activa el **speaker** para simular la explosión y muestra una animación en la pantalla de LEDs. El usuario puede reiniciar la bomba tocando el botón **touch**, lo que la lleva de vuelta al **modo de configuración**.

- **Detalles Técnicos**:
  - El speaker se activa usando `music.play()` o `audio.play()`.
  - La animación en la pantalla de LEDs se implementa usando `display.show()` con imágenes personalizadas.
  - El botón **touch** se verifica para permitir el reinicio.

---

#### **5. Desactivación**
Si el usuario desactiva la bomba durante la cuenta regresiva, el sistema cambia al estado **DESACTIVADA**. En este estado, la cuenta regresiva se detiene, y la bomba vuelve al **modo de configuración**, donde el usuario puede ajustar el tiempo nuevamente o armar la bomba otra vez.

- **Detalles Técnicos**:
  - El botón **touch** se verifica para detectar la desactivación.
  - El sistema reinicia las variables de tiempo y estado para volver al modo de configuración.

---
```py
from microbit import *
import utime

# Definición de estados
STATE_CONFIG = 0
STATE_ARMED = 1
STATE_EXPLOSION = 2
STATE_DISARMED = 3

# Variables de estado
current_state = STATE_CONFIG
start_time = 0
countdown_time = 20  # Tiempo inicial en segundos

while True:
    # Estado CONFIGURACIÓN
    if current_state == STATE_CONFIG:
        display.show(str(countdown_time))  # Mostrar tiempo en pantalla
        if button_a.was_pressed():  # Aumentar tiempo
            countdown_time = min(60, countdown_time + 1)
        if button_b.was_pressed():  # Disminuir tiempo
            countdown_time = max(10, countdown_time - 1)
        if accelerometer.was_gesture("shake"):  # Armar bomba
            current_state = STATE_ARMED
            start_time = utime.ticks_ms()  # Guardar tiempo inicial

    # Estado ARMADA
    elif current_state == STATE_ARMED:
        elapsed_time = utime.ticks_diff(utime.ticks_ms(), start_time) // 1000
        remaining_time = countdown_time - elapsed_time
        display.show(str(remaining_time))  # Mostrar tiempo restante
        if remaining_time <= 0:  # Explosión
            current_state = STATE_EXPLOSION
        if pin_logo.is
```

