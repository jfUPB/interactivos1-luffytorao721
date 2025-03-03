## Explorando funciones del micro:bit

#### **Características del micro:bit**  

- **Procesador ARM Cortex-M4**.  
- **Matriz de LEDs 5x5** programable.  
- **Botones A y B** para interacción táctil.  
- **Acelerómetro y magnetómetro** para detectar movimiento y orientación.  
- **Pines GPIO** para conectar sensores y actuadores.  
- **Bluetooth** para comunicación inalámbrica.  
- **Sensor de temperatura** integrado.  

---

#### **4 Entradas y 4 Salidas del micro:bit**

| **Tipo**      | **Nombre**            | **Descripción**                                                                 |
|---------------|-----------------------|---------------------------------------------------------------------------------|
| **Entrada**   | Botón A               | Detecta cuándo el usuario presiona el botón A.                                  |
| **Entrada**   | Botón B               | Detecta cuándo el usuario presiona el botón B.                                  |
| **Entrada**   | Acelerómetro          | Detecta movimientos como agitación, inclinación o caída.                        |
| **Entrada**   | Sensor de Luz         | Mide la intensidad de la luz ambiental.                                         |
| **Salida**    | Matriz de LEDs        | Muestra patrones, texto o imágenes en una matriz de 5x5 LEDs.                   |
| **Salida**    | Altavoz               | Reproduce sonidos y melodías.                                                  |
| **Salida**    | Pines GPIO (digital)  | Envía señales digitales para controlar LEDs, motores u otros dispositivos.       |
| **Salida**    | Bluetooth             | Permite la comunicación inalámbrica con otros dispositivos.                     |

---

1. **`button_a.is_pressed()`**  
   Detecta si el botón A está siendo presionado.  
   **Ejemplo**:
   ```python
   from microbit import *
   while True:
       if button_a.is_pressed():
           display.scroll("A")
   ```

2. **`accelerometer.current_gesture()`**  
   Detecta gestos como "shake" (agitación) o "tilt" (inclinación).  
   **Ejemplo**:
   ```python
   from microbit import *
   while True:
       if accelerometer.current_gesture() == "shake":
           display.show(Image.HAPPY)
   ```

3. **`display.read_light_level()`**  
   Mide el nivel de luz ambiental.  
   **Ejemplo**:
   ```python
   from microbit import *
   while True:
       light_level = display.read_light_level()
       if light_level < 50:
           display.show(Image.NO)
   ```

---

#### **Funciones de Micropython para Salidas**  

1. **`display.show()`**  
   Muestra imágenes o texto en la matriz de LEDs.  
   **Ejemplo**:
   ```python
   from microbit import *
   display.show(Image.HEART)
   ```

2. **`pin0.write_digital()`**  
   Envía una señal digital (0 o 1) a un pin GPIO.  
   **Ejemplo**:
   ```python
   from microbit import *
   while True:
       pin0.write_digital(1)  # Enciende un LED conectado al pin 0
       sleep(500)
       pin0.write_digital(0)  # Apaga el LED
       sleep(500)
   ```

3. **`music.play()`**  
   Reproduce melodías o sonidos a través del altavoz.  
   **Ejemplo**:
   ```python
   from microbit import *
   music.play(music.NYAN)
   ```

---

