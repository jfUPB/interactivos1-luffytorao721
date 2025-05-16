**¿Por qué es necesario Dev Tunnels y cómo funciona?**
Mi servidor corre en `localhost:3000`, que solo existe dentro de mi máquina. Dev Tunnels me da una URL pública que cualquiera puede abrir; ese túnel toma las peticiones de Internet y las reenvía a mi puerto 3000. Así mi celular, esté donde esté, llega a mi servidor sin que yo abra puertos ni cambie el router.

**¿Por qué uso JSON.stringify en el emisor y JSON.parse en el receptor?**
Socket.IO viaja con texto. Con `JSON.stringify` convierto mi objeto `{x, y}` en una cadena. Cuando llega, `JSON.parse` la vuelve a un objeto usable. Evito concatenar valores manualmente y puedo añadir más campos sin romper el formato.

**Función de touchMoved y el umbral (threshold)**
`touchMoved()` se ejecuta cada vez que arrastro el dedo. Si mando un mensaje en cada pixel se saturaría la red; por eso guardo la última posición y solo emito si me moví más que `threshold`, enviando menos, pero relevantes.

**Otros eventos táctiles en p5.js y posibles usos**
`touchStarted()` para detectar un tap o presionar un botón virtual.
`touchEnded()` para soltar y, por ejemplo, disparar una acción al levantar el dedo.
`doubleClicked()` (no táctil puro, pero útil) para gestos tipo doble tap.
Con ellos puedo hacer botones, sliders o reconocer gestos sencillos sin librerías extras.

**Dev Tunnels vs. IP local**
Con IP local funciona rápido y sin terceros, pero solo dentro de la misma red y a veces el firewall bloquea. Dev Tunnels atraviesa redes y datos móviles, no requiere configurar el router, pero depende de un servicio externo y añade un poco de latencia.
