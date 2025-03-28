### **Código para Tres Semáforos Concurrentes en micro:bit**

```python
from microbit import *
import utime

class Semaforo:
    def __init__(self, x, y, t_rojo, t_amarillo, t_verde):
        self.x = x  # Posición X en la matriz (0-4)
        self.y = y  # Posición Y en la matriz (0-4)
        self.t_rojo = t_rojo * 1000  # Convertir a milisegundos
        self.t_amarillo = t_amarillo * 1000
        self.t_verde = t_verde * 1000
        self.state = "ROJO"
        self.start_time = utime.ticks_ms()
        
    def update(self):
        current_time = utime.ticks_ms()
        elapsed = utime.ticks_diff(current_time, self.start_time)
        
        if self.state == "ROJO":
            display.set_pixel(self.x, self.y, 9)  # Brillo máximo para rojo
            if elapsed > self.t_rojo:
                self.state = "AMARILLO"
                self.start_time = current_time
                
        elif self.state == "AMARILLO":
            display.set_pixel(self.x, self.y, 6)  # Brillo medio para amarillo
            if elapsed > self.t_amarillo:
                self.state = "VERDE"
                self.start_time = current_time
                
        elif self.state == "VERDE":
            display.set_pixel(self.x, self.y, 3)  # Brillo bajo para verde
            if elapsed > self.t_verde:
                self.state = "ROJO"
                self.start_time = current_time

# Crear tres semáforos con tiempos diferentes
s1 = Semaforo(1, 1, 5, 2, 3)  # Semáforo central
s2 = Semaforo(0, 3, 3, 1, 2)  # Semáforo inferior izquierdo
s3 = Semaforo(4, 1, 4, 3, 2)  # Semáforo superior derecho

while True:
    display.clear()  # Limpiar pantalla en cada iteración
    s1.update()
    s2.update()
    s3.update()
    sleep(100)  # Pequeña pausa para evitar parpadeos
```

---

### **Reflexión sobre el Diseño**

**Ventajas de usar clases**:  
La implementación con clases permite encapsular el estado y comportamiento de cada semáforo, haciendo el código modular y escalable. Cada instancia (`s1`, `s2`, `s3`) gestiona sus propios tiempos y transiciones sin interferencias, 
demostrando el principio de **responsabilidad única**. Además, si quisiera añadir un cuarto semáforo, solo necesitaría crear una nueva instancia sin modificar la lógica existente.

**Máquinas de estado como núcleo**:  
La técnica de máquinas de estados (`ROJO → AMARILLO → VERDE`) garantiza que cada semáforo evolucione de manera predecible y concurrente. Aunque el micro:bit tiene limitaciones (como un solo display monocromático), el uso de **brillos 
diferenciados** (9 para rojo, 6 para amarillo, 3 para verde) y posiciones estratégicas en la matriz simula tres sistemas independientes. Este enfoque resuelve el desafío de la concurrencia en un entorno con recursos limitados.

**Desafíos superados**:  
1. **Representación de colores**: Simulé verde y amarillo ajustando el brillo de los LEDs rojos.  
2. **Gestión de tiempos**: Cada semáforo usa su propio `start_time` para evitar desincronizaciones.  
3. **Renderizado no bloqueante**: `display.clear()` en el bucle principal evita "fantasmas" de LEDs.  

**Aplicación futura**:  
Esta estructura sería útil para sistemas de control más complejos (como cruces de tráfico con sensores), donde la concurrencia y la escalabilidad son críticas. En proyectos con p5.js, podría extenderse para controlar múltiples elementos visuales
sincronizados con hardware.  
