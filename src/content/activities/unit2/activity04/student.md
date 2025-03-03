## **Actividad 04: Manipulando Entradas Digitales**  


Decidí Realizar tres experimentos para comprobar el funcionamiento de las entradas digitales del micro:bit (botones **A**, **B** y el **logo táctil**). Aquí están los resultados:

---

#### **Experimento 1: Botón A**  
**¿Qué quería comprobar?**  
Si el botón A puede detectar un pulso y mostrar un mensaje en la matriz de LEDs.  

**Hipótesis:**  
Al presionar el botón A, la matriz mostrará la letra "A"; si no se presiona, no pasará nada.  

**Código:**  
```python
from microbit import *

while True:
    if button_a.is_pressed():
        display.scroll("A")
    else:
        display.clear()
```  

**Resultados:**  
- Al presionar el botón A, la matriz muestra "A".  
- Al soltarlo, la matriz se apaga.  

**Análisis:**  
El botón A funciona como una entrada digital que envía una señal **HIGH** (1) al detectar presión. El método `button_a.is_pressed()` lee este estado y activa la acción correspondiente.  

**Conclusión:**  
El botón A es una entrada digital confiable para acciones simples.  

---

#### **Experimento 2: Botón B**  
**¿Qué quería comprobar?**  
Si el botón B puede cambiar una imagen en la matriz de LEDs.  

**Hipótesis:**  
Al presionar el botón B, la matriz mostrará un ícono feliz (😊).  

**Código:**  
```python
from microbit import *

while True:
    if button_b.is_pressed():
        display.show(Image.HAPPY)
    else:
        display.clear()
```  

**Resultados:**  
- Al presionar el botón B, aparece un ícono feliz.  
- Al soltarlo, la matriz se apaga.  

**Análisis:**  
El botón B también actúa como una entrada digital. La función `button_b.is_pressed()` detecta su estado y activa la salida visual.  

**Conclusión:**  
El botón B permite desencadenar respuestas visuales, útil para interfaces interactivas.  

---

#### **Experimento 3: Logo táctil**  
**¿Qué quería comprobar?**  
Si el logo del micro:bit (touch sensor) puede detectar contacto y mostrar una imagen.  

**Hipótesis:**  
Al tocar el logo, la matriz mostrará un corazón (❤️).  

**Código:**  
```python
from microbit import *

while True:
    if pin_logo.is_touched():
        display.show(Image.HEART)
    else:
        display.clear()
```  

**Resultados:**  
- Al tocar el logo, aparece un corazón.  
- Al retirar el dedo, la matriz se apaga.  

**Análisis:**  
El logo táctil funciona como una entrada **capacitiva** (detecta contacto humano). A diferencia de los botones físicos, no requiere presión.  

**Conclusión:**  
El logo táctil es ideal para interacciones más sutiles, como controles sin botones físicos.  

---

### **Conclusiones Finales**  
1. **Botones A y B** son entradas digitales simples para acciones directas.  
2. **Logo táctil** es una entrada capacitiva, útil para interacciones sin presión física.  
3. Las funciones `button_a.is_pressed()` y `pin_logo.is_touched()` son clave para manejar entradas.  
4. Estos experimentos demuestran cómo integrar entradas físicas en proyectos interactivos, como videojuegos o instalaciones artísticas.  

