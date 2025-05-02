### **Bitácora - Actividad 01: Preparación del entorno**  

#### **1. Error al ejecutar `npm install` y `npm start`**  
- **Problema**: Ambos comandos fallaron porque no se encontró el archivo `package.json` en la ubicación actual (`C:\Users\B15520lest`).  
- **Causa**: No navegué a la carpeta del proyecto clonado (donde sí está el `package.json`).  
- **Solución**: Usar `cd ruta/de/la/carpeta/del/proyecto` antes de ejecutar los comandos.  

#### **2. ¿Qué hace `npm install`?**  
Instala las dependencias del proyecto (como Socket.IO y Express) listadas en el `package.json`. Sin esto, el servidor no puede funcionar.  

#### **3. Mensaje esperado con `npm start`**  
Si se ejecuta correctamente, debería aparecer:  
```  
Servidor escuchando en http://localhost:3000  
```  
Esto confirma que el servidor Node.js está activo.  

#### **4. Comportamiento de las páginas (cuando el servidor funciona)**  
- **page1**: Círculo rojo y texto "Página 1".  
- **page2**: Círculo azul y texto "Página 2".  
- **Interacción**: Al mover una ventana, la otra página refleja el movimiento en tiempo real.  

#### **5. Mensajes en el servidor (éxito)**  
- Al abrir las páginas:  
  ```  
  Cliente conectado: [ID único]  
  ```  
- **Consola del navegador (F12)**: Muestra eventos `positionUpdated` con coordenadas.  
![image](https://github.com/user-attachments/assets/5c137f59-ab09-4daf-8c9c-71691fc4235e)
