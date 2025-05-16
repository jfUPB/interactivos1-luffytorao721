### ** Análisis del Servidor (server.js)**  

![image](https://github.com/user-attachments/assets/f596db35-ec01-45ab-8cf5-6870f9da1830)

#### **1. Dependencias y módulos**  
- **Ventajas de los módulos**: Evitan reinventar la rueda. Ejemplo: `express` simplifica rutas HTTP, `socket.io` maneja WebSockets por nosotros.  
- **Reflexión**: Escribir todo desde cero sería como construir un auto sin piezas prefabricadas: posible, pero ineficiente.  

#### **2. Creación del servidor**  
- `app = express()`: Es el "cerebro" del servidor por así decirlo.  
- `io = socketIO(server)`: Añade superpoderes de comunicación en tiempo real.  
- **Experimento**: Cambiar `'views'` por `'archivos_cliente'` (inexistente) → Error 404. Confirmé que Express sirve archivos **solo** desde la carpeta especificada.  

#### **3. Estado global**  
- `page1` y `page2` guardan posiciones. **Importante**: El servidor es el "árbitro" que comparte datos entre clientes.  

#### **4. Rutas HTTP**  
- **Prueba**: Cambiar `/page1` a `/pagina_uno` → La URL **debe coincidir** exactamente con lo definido en `app.get()`.  
- **LO que se traduce en*: Las rutas son como "direcciones postales": si las cambias, el servidor no encuentra la "casa".  

#### **5. Conexiones Socket.IO**  
- **Logs de conexión**:  
  - Al abrir `page1`: `A user connected - ID: abc123`.  
  - Al abrir `page2`: ID diferente (`def456`).  
  - Al cerrar `page1`: `User disconnected - ID: abc123`.  
- **Key insight**: Cada pestaña tiene un ID único, incluso en el mismo navegador.  

#### **6. Mensajes entre clientes**  
- **Flujo**:  
  1. `page1` envía `win1update` → Servidor actualiza `page1`.  
  2. Servidor usa `broadcast.emit('getdata')` → Notifica a **todos menos al emisor**.  
- **Experimento clave**:  
  - Cambiar `broadcast.emit` a `socket.emit` → Solo el emisor recibe el mensaje (`page2` no se actualiza).  
  - **Aprendizaje**: `broadcast` es esencial para comunicación grupal.  

#### **7. Puerto del servidor**  
- Cambiar `port` a `3001` → Servidor escucha en `http://localhost:3001`.  
- **Error común**: Olvidar actualizar la URL en el navegador. ¡El puerto en `listen()` y la URL deben coincidir!  


 confundido con `broadcast` 
 ![image](https://github.com/user-attachments/assets/eb4b166e-71a4-4d6a-a471-2559b196147f)

