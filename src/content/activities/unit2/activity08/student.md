### **Actividad 08: Analizando un Programa con Máquinas de Estados**

En esta actividad, analizaremos el código proporcionado para identificar **estados**, **eventos** y **acciones**. El programa utiliza una **máquina de estados** para controlar el comportamiento de dos píxeles en la matriz de LEDs del micro:bit.

---

### **Código Analizado**

```python
from microbit import *
import utime

class Pixel:
    def __init__(self, pixelX, pixelY, initState, interval):
        self.state = "Init"
        self.startTime = 0
        self.interval = interval
        self.pixelX = pixelX
        self.pixelY = pixelY
        self.pixelState = initState

    def update(self):
        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            display.set_pixel(self.pixelX, self.pixelY, self.pixelState)

        elif self.state == "WaitTimeout":
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > self.interval:
                self.startTime = utime.ticks_ms()
                if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
                display.set_pixel(self.pixelX, self.pixelY, self.pixelState)

pixel1 = Pixel(0, 0, 0, 1000)
pixel2 = Pixel(4, 4, 0, 500)

while True:
    pixel1.update()
    pixel2.update()
```

---

### **Descripción del Funcionamiento**

El programa define una clase `Pixel` que controla el estado de un píxel en la matriz de LEDs del micro:bit. Cada píxel tiene dos estados principales: **"Init"** y **"WaitTimeout"**. El píxel cambia entre encendido (`9`) y apagado (`0`) en intervalos de tiempo específicos.

---

### **Identificación de Estados, Eventos y Acciones**

#### **1. Estados**
Los estados son momentos en los que el programa espera a que ocurra algo. En este caso, los estados son:

- **"Init"**: Estado inicial del píxel. Aquí se configura el tiempo inicial y se establece el estado del píxel en la matriz de LEDs.
- **"WaitTimeout"**: Estado en el que el píxel espera a que transcurra el tiempo especificado (`interval`) para cambiar su estado (encendido/apagado).

---

#### **2. Eventos**
Los eventos son las condiciones que el programa verifica para decidir si debe cambiar de estado o realizar una acción. En este caso, los eventos son:

- **Inicialización del píxel**: Cuando el píxel está en el estado `"Init"`, el programa configura el tiempo inicial y cambia al estado `"WaitTimeout"`.
- **Transcurrir el intervalo de tiempo**: Cuando el píxel está en el estado `"WaitTimeout"`, el programa verifica si ha transcurrido el tiempo especificado (`interval`). Si es así, cambia el estado del píxel (encendido/apagado).

---

#### **3. Acciones**
Las acciones son las operaciones que el programa ejecuta en respuesta a un evento. En este caso, las acciones son:

- **Configurar el tiempo inicial**: En el estado `"Init"`, el programa guarda el tiempo actual (`utime.ticks_ms()`) en `self.startTime`.
- **Cambiar el estado del píxel**: En el estado `"WaitTimeout"`, el programa cambia el estado del píxel entre encendido (`9`) y apagado (`0`).
- **Actualizar la matriz de LEDs**: En ambos estados, el programa actualiza la matriz de LEDs usando `display.set_pixel()`.

---

### **Respuestas a las Preguntas**

#### **1. ¿Puedes identificar algún estado?**
Sí, los estados identificados son:
- **"Init"**: Estado inicial donde se configura el píxel.
- **"WaitTimeout"**: Estado donde el píxel espera a que transcurra el intervalo de tiempo para cambiar su estado.

#### **2. ¿Puedes identificar algún evento?**
Sí, los eventos identificados son:
- **Inicialización del píxel**: Ocurre cuando el píxel está en el estado `"Init"`.
- **Transcurrir el intervalo de tiempo**: Ocurre cuando el píxel está en el estado `"WaitTimeout"` y el tiempo especificado ha pasado.

#### **3. ¿Puedes identificar alguna acción?**
Sí, las acciones identificadas son:
- **Configurar el tiempo inicial**: Guarda el tiempo actual en `self.startTime`.
- **Cambiar el estado del píxel**: Alterna entre encendido (`9`) y apagado (`0`).
- **Actualizar la matriz de LEDs**: Usa `display.set_pixel()` para reflejar el estado actual del píxel.

---

### **Conclusión**

Este programa es un ejemplo claro de una **máquina de estados**, donde el comportamiento del píxel está determinado por su estado actual y los eventos que ocurren. Los estados definen qué hace el programa en cada momento, los eventos son las condiciones que deben cumplirse para cambiar de estado, y las acciones son las operaciones que se ejecutan en respuesta a esos eventos.

Este enfoque es útil para programar sistemas que requieren un control preciso y modular, como animaciones, juegos o sistemas de notificación. 😊🎮
