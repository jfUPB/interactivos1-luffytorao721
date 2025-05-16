### **Actividad 06: Generando Salidas Visuales**

En esta actividad, experimentaremos con la pantalla LED del micro:bit para mostrar diferentes imágenes y textos. El objetivo es crear un programa que muestre al menos **3 imágenes o textos diferentes** en la pantalla del micro:bit.

---

### **Código en Micropython**

```python
from microbit import *

# Muestra una secuencia de imágenes y textos
while True:
    # Muestra una cara feliz
    display.show(Image.HAPPY)
    sleep(2000)  # Espera 2 segundos

    # Muestra un corazón
    display.show(Image.HEART)
    sleep(2000)  # Espera 2 segundos

    # Muestra un texto
    display.scroll("Luffy")
    sleep(1000)  # Espera 1 segundo

    # Muestra una mariposa
    display.show(Image.BUTTERFLY)
    sleep(2000)  # Espera 2 segundos
```

---

### **Explicación del Funcionamiento**

1. **Importación de Módulos**:
   - **`from microbit import *`**: Importa todas las funciones y clases necesarias para programar el micro:bit.

2. **Bucle Principal**:
   - El programa entra en un bucle infinito (`while True`), lo que significa que se ejecutará continuamente.

3. **Mostrar una Cara Feliz**:
   - **`display.show(Image.HAPPY)`**: Muestra una imagen de una cara feliz en la matriz de LEDs.
   - **`sleep(2000)`**: Pausa la ejecución del programa durante 2 segundos para que la imagen sea visible.

4. **Mostrar un Corazón**:
   - **`display.show(Image.HEART)`**: Muestra una imagen de un corazón en la matriz de LEDs.
   - **`sleep(2000)`**: Pausa la ejecución del programa durante 2 segundos.

5. **Mostrar un Texto**:
   - **`display.scroll("Luffy")`**: Desplaza el texto "Luffy" en la matriz de LEDs.
   - **`sleep(1000)`**: Pausa la ejecución del programa durante 1 segundo.

6. **Mostrar una Mariposa**:
   - **`display.show(Image.BUTTERFLY)`**: Muestra una imagen de una mariposa en la matriz de LEDs.
   - **`sleep(2000)`**: Pausa la ejecución del programa durante 2 segundos.

---

### **Resultados Esperados**

- El micro:bit mostrará una secuencia de imágenes y textos en la siguiente orden:
  1. Una cara feliz (`Image.HAPPY`) durante 2 segundos.
  2. Un corazón (`Image.HEART`) durante 2 segundos.
  3. El texto "Hola" desplazándose en la pantalla.
  4. Una mariposa (`Image.BUTTERFLY`) durante 2 segundos.

- Después de mostrar la mariposa, el ciclo se repetirá indefinidamente.

---

### **Análisis de funciones**

- **`display.show()`**: Esta función es útil para mostrar imágenes predefinidas o personalizadas en la matriz de LEDs.
- **`display.scroll()`**: Esta función permite mostrar texto desplazándose en la pantalla, lo que es ideal para mensajes cortos.
- **`sleep()`**: Introduce retardos en el programa para que las imágenes y textos sean visibles durante un tiempo determinado.

---
