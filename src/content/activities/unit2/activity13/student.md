**Bitácora del Diseño: La Bomba que Aprendió a Latir**  

Cuando diseñé la máquina de estados para la bomba temporizada, no estaba creando solo código, sino dándole vida a un personaje digital con pulso propio. **Al principio**, imaginé la bomba como un ser de tres personalidades: la *calculadora fría* 
(modo configuración), la *corredora contra el tiempo* (cuenta regresiva) y la *drama queen* (explosión). **Pero pronto descubrí** que mi creación tenía más matices: el botón touch era su "punto débil", y el gesto shake su grito de guerra.  

**El primer dilema creativo** fue el tiempo. Quería que los segundos latieran como un corazón, así que implementé `show_time()` no solo para mostrar números, sino para crear suspense con pausas calculadas (200ms de delay,
¡justo lo que tarda un parpadeo humano!). **Sin embargo**, la bomba se resistía: a veces el gesto shake no se detectaba, como si estuviera de mal humor. Solucioné esto añadiendo un "ritual de activación" (agitarla como si fuera un spray).  

**La explosión fue mi obra maestra**. En lugar del típico "bip", elegí `music.BA_DING` seguido de un cráneo (Image.SKULL), porque toda buena historia necesita un final memorable. **Pero me di cuenta** de que faltaba emoción en la cuenta regresiva: 
¿y si en los últimos 5 segundos los LEDs parpadearan como corazones nerviosos?  

**Para la próxima iteración**, planeo:  
1) **Darle voz**: Usar el speaker para susurrar "tic-tac" acelerándose,  
2) **Personalidad reactiva**: Que el brillo de los LEDs cambie según la presión al tocar los botones, y  
3) **Un easter egg**: Si presionas A+B simultáneamente 3 veces, mostraría un emoji guiño (¡sabotaje cómico!).  

**Al final**, esta bomba no es solo un circuito, es un personaje en mi escape room digital. Y como todo buen personaje, aún tiene secretos por revelar.  
