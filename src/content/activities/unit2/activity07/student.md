### **Actividad 07: Produciendo Sonidos con el Micro:bit**

En esta actividad, exploraremos cómo generar sonidos utilizando el **speaker** o **buzzer** del micro:bit. El objetivo es crear un programa que genere al menos **2 sonidos diferentes**.

---

### **Código en Micropython**

```python
from microbit import *
import music

# Reproduce dos sonidos diferentes
while True:
    # Reproduce la melodía "NYAN"
    music.play(music.NYAN)
    sleep(1000)  # Espera 1 segundo

    # Reproduce una nota musical (Do)
    music.pitch(262, 500)  # Frecuencia de 262 Hz (Do) durante 500 ms
    sleep(1000)  # Espera 1 segundo
```

---

### **Explicación del Funcionamiento**

1. **Importación de Módulos**:
   - **`from microbit import *`**: Importa todas las funciones y clases necesarias para programar el micro:bit.
   - **`import music`**: Importa el módulo `music`, que proporciona funciones para generar sonidos y melodías.

2. **Bucle Principal**:
   - El programa entra en un bucle infinito (`while True`), lo que significa que se ejecutará continuamente.

3. **Reproducir la Melodía "NYAN"**:
   - **`music.play(music.NYAN)`**: Reproduce la melodía predefinida "NYAN" (el famoso tema de Nyan Cat).
   - **`sleep(1000)`**: Pausa la ejecución del programa durante 1 segundo después de reproducir la melodía.

4. **Reproducir una Nota Musical (Do)**:
   - **`music.pitch(262, 500)`**: Genera una nota musical con una frecuencia de **262 Hz** (Do) durante **500 ms**.
   - **`sleep(1000)`**: Pausa la ejecución del programa durante 1 segundo después de reproducir la nota.

---

### **Resultados Esperados**

- El micro:bit reproducirá la melodía "NYAN" y luego una nota musical (Do) en un ciclo infinito.
- Cada sonido estará separado por una pausa de 1 segundo.

---

### **Análisis**

- **`music.play()`**: Esta función reproduce melodías predefinidas o personalizadas. En este caso, usé la melodía "NYAN", que está incluida en el módulo `music`.
- **`music.pitch()`**: Esta función genera una nota musical con una frecuencia específica (en Hz) y una duración (en ms). Aquí, usamos una frecuencia de **262 Hz** para producir la nota Do.
- **`sleep()`**: Introduce retardos en el programa para separar los sonidos y hacerlos más perceptibles.

---

### **Documentación del Código**

#### **Funciones Utilizadas**
1. **`music.play(melody)`**:
   - **Descripción**: Reproduce una melodía predefinida o personalizada.
   - **Parámetros**:
     - `melody`: La melodía a reproducir (por ejemplo, `music.NYAN`).
   - **Ejemplo**:
     ```python
     music.play(music.NYAN)
     ```

2. **`music.pitch(frequency, duration)`**:
   - **Descripción**: Genera una nota musical con una frecuencia y duración específicas.
   - **Parámetros**:
     - `frequency`: La frecuencia de la nota en Hz (por ejemplo, 262 Hz para Do).
     - `duration`: La duración de la nota en milisegundos (por ejemplo, 500 ms).
   - **Ejemplo**:
     ```python
     music.pitch(262, 500)  # Reproduce Do durante 500 ms
     ```

3. **`sleep(ms)`**:
   - **Descripción**: Pausa la ejecución del programa durante un tiempo específico.
   - **Parámetros**:
     - `ms`: El tiempo de pausa en milisegundos (por ejemplo, 1000 ms = 1 segundo).
   - **Ejemplo**:
     ```python
     sleep(1000)  # Pausa de 1 segundo
     ```

---
