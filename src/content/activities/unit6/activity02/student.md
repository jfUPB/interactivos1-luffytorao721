### Unidad 6: Viaje de los datos (Cliente-Servidor)**  

#### **1. Internet como red física**  
- **Analogía**: Internet son carreteras (cables) que conectan "lugares" (servidores). Mi Wi-Fi es la rampa de acceso.  
- **Sin conexión**: Sin Wi-Fi/cable, mi "vehículo" (navegador) no puede llegar a ningún servidor.  

#### **2. Modelo Cliente-Servidor**  
- **Ejemplo real**: En un restaurante, **yo** (cliente) pido comida (datos) al **mesero** (servidor).  
- **Digital**: Navegador (cliente) pide páginas web a un servidor (como Apache o Node.js).  

#### **3. Partes de una URL**  
- **Ejemplo**: `https://github.com/user/repo`  
  - Protocolo: `https://` (reglas de comunicación).  
  - Dominio: `github.com` (dirección del servidor).  
  - Ruta: `/user/repo` (ubicación específica del recurso).  
- **Sin ruta**: El servidor envía la página por defecto (ej: `index.html`).  

#### **4. HTTP vs. Serial**  
- **Similitudes**: Ambos son protocolos (reglas para comunicarse).  
- **Diferencias**: HTTP incluye metadatos (ej: `Content-Type`), mientras que el serial envía bytes crudos.  
- **Complejidad**: HTTP debe manejar múltiples tipos de datos (HTML, imágenes, etc.), no solo números.  

#### **5. HTML, CSS y JS**  
- **Login web**:  
  - **HTML**: Campos de texto y botón (`<input>`, `<button>`).  
  - **CSS**: Color del botón (`background-color: blue;`).  
  - **JS**: Validar contraseña antes de enviar (`if (password.length > 0)`).  

#### **6. JS basado en eventos**  
- **Ventajas**:  
  - Eficiente: Solo ejecuta código cuando ocurre algo (ej: clic).  
  - Menos carga: No redibuja la página innecesariamente (como haría `draw()`).  

#### **7. Node.js y Socket.IO**  
- **Node.js**: Permite usar JS en el servidor. Ventaja: Mismo lenguaje en frontend y backend.  
- **Socket.IO vs HTTP**:  
  - **HTTP**: Como correos (petición-respuesta).  
  - **Socket.IO**: Como llamada telefónica (comunicación instantánea).  
- **Aplicaciones**: Chats, juegos multijugador, actualizaciones en tiempo real (ej: Google Docs).  

---  
**eebden**: Entendí cómo los datos viajan entre cliente y servidor, y por qué Socket.IO es clave para aplicaciones interactivas.  

