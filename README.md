# Fortaleza Roja

Shooter en primera persona para el navegador, al estilo de los clásicos de los 90, **pensado para que lo puedan jugar cómodas las personas con dislexia**. Todo está en un solo archivo `index.html`: no usa librerías, ni imágenes, ni archivos de audio. Los gráficos y los sonidos se generan con código.

**Jugar:** https://jesus1942.github.io/fortaleza-roja/

---

## Por qué un juego así, en el navegador y para personas con dislexia

Muchos juegos le piden al jugador que lea todo el tiempo: menús largos, misiones explicadas en párrafos, avisos en letra chica, textos que aparecen y desaparecen rápido. Para una persona con dislexia eso no es un detalle. Leer bajo presión cansa, hace perder información y deja a la persona en desventaja en algo que debería ser solo diversión.

Un shooter en primera persona es una buena base para resolver eso, porque casi todo lo que importa ya se entiende sin leer: hacia dónde ir, qué es un enemigo, qué es un botiquín, cuándo te están atacando. La idea de Fortaleza Roja es llevar eso al máximo. El juego te cuenta lo que pasa con **imágenes, colores, íconos y sonido**, y el texto que queda es corto, claro y opcional.

Elegí el **navegador** porque es la puerta de entrada más fácil. No hay que instalar nada ni tener una computadora potente. Funciona en la compu de la escuela, en una notebook vieja o en el celular, y se comparte con un link. Así cualquiera lo puede probar.

Y elegí un juego **de acción** a propósito. Las herramientas de accesibilidad para la dislexia suelen ser educativas o de lectura. Este proyecto muestra que también se puede hacer un juego rápido y desafiante, donde lo que cuenta son los reflejos y la orientación, y no la velocidad de lectura.

## Qué hace el juego para la dislexia

| Mejora | Para qué sirve |
|---|---|
| **Modo lectura fácil** (activado de entrada) | Usa la tipografía **Lexend**, diseñada para leer con menos esfuerzo. Saca las mayúsculas forzadas y suma espacio entre letras, palabras y renglones. |
| **Íconos en la barra de estado** | Corazón para la vida, balas para la munición, estrella para los puntos, bandera para la zona. Se entiende de un vistazo, sin leer. |
| **Avisos nítidos con ícono** | Cada aviso muestra el dibujo del objeto (botiquín, caja de balas, cristal) y un color: verde si es bueno, rojo si es peligro, celeste si es un premio. Se leen sobre un fondo sólido, en frases cortas, y quedan en pantalla 3 segundos o más. |
| **Leer en voz alta** (opcional) | La voz del navegador dice en español los avisos importantes: zona nueva, vida baja, sin balas. |
| **Escuchar instrucciones** | Un botón del menú lee en voz alta cómo se juega. |
| **Indicador de daño** | Un arco rojo en pantalla muestra de qué lado vino el golpe, sin texto. |
| **Sin parpadeos** | La vida baja se marca con un borde rojo fijo y un número rojo, nunca con texto que titila. |
| **Teclas dibujadas como teclas** | Los controles se muestran como teclas reales (`W` `A` `S` `D`), más fáciles de reconocer que una lista de letras. |

Las opciones quedan guardadas en el navegador y se pueden cambiar desde el menú o la pausa.

## Cómo funciona por dentro (el algoritmo)

### 1. Raycasting: un mundo 2D que se ve en 3D
El mapa es una grilla plana de casillas: pared o espacio libre. Para dibujar la pantalla, por cada columna de píxeles (480 por cuadro) se lanza un **rayo** desde el jugador en la dirección de esa columna. El rayo avanza casilla por casilla con el algoritmo **DDA** (*Digital Differential Analyzer*), que salta directo al próximo borde de la grilla, hasta chocar con una pared.

- La **distancia** del choque define la altura de la pared en pantalla: más lejos, más baja.
- Se usa la distancia **perpendicular** a la cámara, no la real, para evitar el efecto "ojo de pez".
- El punto exacto donde pegó el rayo elige qué **columna de la textura** se dibuja.
- Las caras en un sentido se oscurecen un poco y todo se funde con una **niebla** según la distancia, para dar profundidad.
- **Mirar arriba y abajo** desplaza la línea del horizonte (técnica *y-shearing*). Es el mismo truco que usaban los juegos de la época.

### 2. Sprites y profundidad
Enemigos, objetos y proyectiles son imágenes planas (*sprites*). Cada uno se transforma al espacio de la cámara con la inversa de la matriz de dirección del jugador, se ordena del más lejano al más cercano y se dibuja columna por columna. Mientras se dibujan las paredes se guarda un **z-buffer** (la distancia de cada columna), así un sprite detrás de una pared no se ve.

### 3. Mundo infinito por bloques
El mundo no está guardado: se **genera a medida que caminás**, en bloques de 16 × 16 casillas.

- Cada bloque usa una semilla calculada con un **hash** de sus coordenadas y de la semilla de la partida. Si volvés a un lugar, es el mismo laberinto.
- Dentro del bloque hay una grilla de 4 × 4 celdas con pasillos de 3 casillas de ancho. Un **árbol de expansión** (búsqueda en profundidad con semilla) conecta todas las celdas.
- Después se agregan conexiones extra para que **no queden callejones sin salida**, y a veces se funden 4 celdas en un hall de 7 × 7.
- Cada bloque es dueño de su muro este y sur, y abre al menos una puerta en cada uno. Así el mundo infinito queda **siempre conectado**.
- Solo se generan los bloques que el rayo o el jugador tocan, y se guardan en un `Map`.

### 4. Dificultad por distancia
La **zona** es la distancia en bloques al punto de partida (anillos alrededor del origen). Cuanto más lejos, más enemigos aparecen, con más vida, más daño y más frecuencia de ataque. Llegar a una zona nueva da puntos extra. Si limpiás un área, aparecen refuerzos **fuera de tu vista**. Antes de crear uno se verifica que no haya línea de visión, para que nada aparezca delante tuyo.

### 5. Inteligencia de los enemigos
Cada enemigo es una pequeña **máquina de estados**: *quieto → persigue → ataca → dolor → muerto*. Para saber si te ve, se recorre la línea entre los dos en pasos cortos buscando paredes. Hay tres tipos: uno que combina garras y bolas de fuego, uno flotante que mantiene distancia y dispara, y uno grande y lento que pega fuerte. Los enemigos muy lejanos quedan dormidos para ahorrar cálculo.

### 6. Disparos
Al disparar se calcula a qué distancia está la pared en esa dirección. Después se busca el enemigo más cercano que quede dentro del ángulo de tiro, antes de la pared y a la altura de la mira (porque se puede mirar arriba y abajo). La escopeta tira 7 perdigones con dispersión.

### 7. Sonido sintetizado
No hay archivos de audio. Cada sonido (disparo, golpe, recoger un objeto) se arma en vivo con **Web Audio**: osciladores y ruido filtrado con envolventes cortas.

## Controles

| Entrada | Acción |
|---|---|
| `W` `A` `S` `D` / flechas | caminar y moverse de costado |
| Mouse / touchpad | girar y mirar arriba o abajo mientras caminás (clic para capturarlo) |
| Clic / `Espacio` | disparar |
| `Q` `E` | girar con el teclado |
| `R` `F` `C` | mirar arriba, abajo y al centro |
| `1` `2` | pistola y escopeta |
| `Shift` | correr |
| `M` | mapa |
| Gamepad | stick izquierdo camina, stick derecho gira, gatillo dispara, Start pausa |
| Táctil | joystick a la izquierda, deslizar a la derecha para girar, botones FUEGO y ARMA |

Todo se puede hacer al mismo tiempo: avanzar, moverse de costado, girar, mirar arriba o abajo y disparar.

> **Touchpad:** muchos sistemas apagan el touchpad mientras se aprieta una tecla. Si no gira mientras caminás, desactivá «deshabilitar touchpad al escribir».

## Correrlo localmente

Abrí `index.html` en el navegador. No necesita instalación ni compilación. Cada `push` a `main` se publica solo en GitHub Pages (ver `.github/workflows/pages.yml`).

## Contribuyentes

- **Jesús Olguín** ([@jesus1942](https://github.com/jesus1942)): autor del proyecto. Idea, enfoque de accesibilidad para personas con dislexia, dirección del juego, diseño y pruebas.
- Desarrollado con la asistencia de **Claude** (Anthropic).

¿Querés sumarte? Abrí un *issue* con ideas, sobre todo si tenés dislexia y querés contar qué te ayuda o qué te molesta al jugar.

---

Jesús Olguín · Domotics & IoT Solutions
