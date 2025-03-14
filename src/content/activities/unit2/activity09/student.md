### **Código en Micropython (Semáforo con Coordenadas)**

```python
from microbit import *
import utime

class Semaforo:
    def __init__(self):
        self.state = "Rojo"  # Estado inicial
        self.startTime = utime.ticks_ms()  # Tiempo inicial

    def update(self):
        if self.state == "Rojo":
            # Acción: Encender luz roja, apagar amarillo y verde
            display.clear()  # Limpiar la pantalla
            display.set_pixel(1, 2, 9)  # Rojo en (1, 2)
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > 3000:  # 5 segundos
                self.state = "Amarillo"  # Cambiar a verde
                self.startTime = utime.ticks_ms()  # Reiniciar tiempo

        elif self.state == "Amarillo":
            # Acción: Encender luz amarilla, apagar rojo y verde
            display.clear()  # Limpiar la pantalla
            display.set_pixel(2, 2, 9)  # Amarillo en (2, 2)
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > 1000:  # 2 segundos
                self.state = "Verde"  # Cambiar a rojo
                self.startTime = utime.ticks_ms()  # Reiniciar tiempo

        elif self.state == "Verde":
            # Acción: Encender luz verde, apagar rojo y amarillo
            display.clear()  # Limpiar la pantalla
            display.set_pixel(3, 2, 9)  # Verde en (3, 2)
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > 3000:  # 3 segundos
                self.state = "Rojo"  # Cambiar a amarillo
                self.startTime = utime.ticks_ms()  # Reiniciar tiempo

        

# Crear una instancia del semáforo
semaforo = Semaforo()

while True:
    semaforo.update()  # Actualizar el estado del semáforo
    sleep(100)  # Pequeña pausa para evitar sobrecarga
```

---

### **Explicación del Código**

1. **Coordenadas de los LEDs**:
   - **Rojo**: Se enciende en la posición `(1, 2)`.
   - **Amarillo**: Se enciende en la posición `(2, 2)`.
   - **Verde**: Se enciende en la posición `(3, 2)`.

2. **Funciones Utilizadas**:
   - **`display.clear()`**: Limpia la pantalla antes de encender un nuevo LED.
   - **`display.set_pixel(x, y, brightness)`**: Enciende un LED en la posición `(x, y)` con un brillo específico (`9` para encendido, `0` para apagado).

3. **Lógica de Cambio de Estado**:
   - El semáforo cambia de estado después de que transcurre el tiempo especificado:
     - **Rojo**: 5 segundos.
     - **Verde**: 5 segundos.
     - **Amarillo**: 2 segundos.

---

### **Funcionamiento del Código**

1. **Estado Inicial (Rojo)**:
   - El semáforo comienza en rojo y enciende el LED en `(1, 2)`.
   - Después de 5 segundos, cambia al estado **Verde**.

2. **Estado Verde**:
   - El semáforo enciende el LED en `(3, 2)`.
   - Después de 5 segundos, cambia al estado **Amarillo**.

3. **Estado Amarillo**:
   - El semáforo enciende el LED en `(2, 2)`.
   - Después de 2 segundos, vuelve al estado **Rojo**.

---

### **Prueba del Código**

1. **Carga el Código en el micro:bit**:
   - Conecta el micro:bit a la computadora y carga el código usando el editor de Micropython.

2. **Observa el Comportamiento**:
   - El semáforo debería cambiar de color en el siguiente orden:
     - Rojo (5 segundos) → Verde (5 segundos) → Amarillo (2 segundos) → Rojo (5 segundos) → ...

---

### **Resultados Esperados**

- **Rojo**: El LED en `(1, 2)` se enciende durante 5 segundos.
- **Verde**: El LED en `(3, 2)` se enciende durante 5 segundos.
- **Amarillo**: El LED en `(2, 2)` se enciende durante 2 segundos.

---

