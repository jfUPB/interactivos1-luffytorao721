

## **Análisis del código `server.js`**

###  **Setup inicial del servidor**

```js
const express = require('express');
const http = require('http');
const socketIO = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIO(server);
const port = 3000;
```

#### ✅ ¿Qué está pasando aquí?

1. **`express`**: crea el servidor web.
2. **`http`**: permite crear un servidor base compatible con WebSockets.
3. **`socket.io`**: añade soporte para comunicación en tiempo real.
4. Se establece el **puerto 3000** como punto de acceso local (que será publicado por Dev Tunnels).

---

###  **Servir archivos estáticos**

```js
app.use(express.static('public'));
```

#### ✅ ¿Qué hace esto?

Esta línea le dice a Express que **sirva automáticamente los archivos de la carpeta `public/`**. Es decir, si visitas `http://localhost:3000/desktop/index.html`, el servidor busca directamente `public/desktop/index.html`.

#### 🔁 Comparación con `app.get()` (Unidad 6):

* `app.get('/ruta', ...)` sirve rutas **específicas y manuales**.
* `express.static()` **automatiza** todo el proceso: se sirve todo el contenido estático según las carpetas y archivos.

---

### 🔌 **Conexión de clientes vía WebSockets**

```js
io.on('connection', (socket) => {
    console.log('New client connected - ID:', socket.id);

    socket.on('message', (message) => {
        console.log(`Received message => ${message} from ID: ${socket.id}`);
        socket.broadcast.emit('message', message);
    });

    socket.on('disconnect', () => {
        console.log('Client disconnected - ID:', socket.id);
    });
});
```

#### ✅ Explicación paso a paso:

1. **`io.on('connection')`**: se ejecuta cada vez que **un cliente se conecta** al servidor.
2. El parámetro `socket` representa la conexión de ese cliente específico.
3. **`socket.on('message')`**:

   * Se activa cuando ese cliente **envía un mensaje** (por ejemplo, desde el móvil).
   * El servidor **recibe y retransmite** el mensaje a los **otros clientes**, usando `socket.broadcast.emit(...)`.
4. **`socket.broadcast.emit('message', message)`**:

   * **Reenvía el mensaje a todos los clientes conectados excepto al que lo envió**.
5. **`socket.on('disconnect')`**:

   * Se ejecuta cuando el cliente se desconecta.

---

### ▶️ **Iniciar el servidor**

```js
server.listen(port, () => {
    console.log(`Server is listening on http://localhost:${port}`);
});
```

El servidor empieza a **escuchar conexiones entrantes en el puerto 3000**. Cuando lo hace correctamente, imprime un mensaje de confirmación.

---

## 🧩 **Respuestas a preguntas de reflexión**

### ❓ ¿Cuál es la función principal de `express.static('public')`?

Sirve automáticamente los archivos estáticos (HTML, JS, CSS, imágenes) ubicados en la carpeta `public`. Es mucho más simple que definir manualmente cada ruta con `app.get(...)`, especialmente para proyectos con muchas páginas o archivos.

---

### ❓ Flujo del mensaje táctil

1. **Cliente móvil (JS)**: detecta un toque y envía un mensaje al servidor con `socket.emit('message', datos)`.
2. **Servidor (`server.js`)**:

   * Recibe el mensaje mediante `socket.on('message', callback)`.
   * Lo reenvía a los demás clientes con `socket.broadcast.emit('message', datos)`.
3. **Cliente escritorio**: está suscrito al evento `socket.on('message', callback)` y lo recibe.

🔁 **¿Por qué usar `socket.broadcast.emit`?**

Porque queremos que **todos los demás** clientes (escritorio u otros móviles) reciban el mensaje, **excepto el que lo envió** (el móvil).

* `socket.emit`: solo al cliente actual.
* `io.emit`: a **todos**, incluido el que envió.
* `socket.broadcast.emit`: a **todos menos el que envió**. ¡Perfecto para nuestro caso!

---

### ❓ ¿Qué pasa si conectas dos escritorios y un móvil?

Si el móvil envía un mensaje táctil:

* **Ambos escritorios recibirán el mensaje** (porque no lo originaron).
* **El móvil no lo recibe de vuelta** (ya lo tiene localmente).

Esto se debe a que se usa `socket.broadcast.emit(...)`, que **excluye al cliente emisor**.

---

### ❓ ¿Para qué sirven los `console.log`?

1. Informan cuando **alguien se conecta o desconecta**, lo cual ayuda a monitorear el estado del servidor.
2. Muestran los **mensajes recibidos** y quién los envió (`socket.id`).
3. Son **útiles para depuración** y verificar que el flujo de mensajes funcione correctamente.

---

## ✅ **Resumen general del `server.js`**

| Sección                   | Función principal                                      |
| ------------------------- | ------------------------------------------------------ |
| `express.static()`        | Sirve archivos cliente desde `public/` automáticamente |
| `io.on('connection')`     | Gestiona nuevos clientes y su comunicación             |
| `socket.on('message')`    | Recibe mensajes del cliente                            |
| `socket.broadcast.emit()` | Retransmite mensajes a otros clientes                  |
| `server.listen()`         | Inicia el servidor en el puerto 3000                   |

---


