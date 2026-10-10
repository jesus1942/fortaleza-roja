# Fortaleza Roja

FPS retro en primera persona hecho con **raycasting** sobre `<canvas>`, en un único
archivo HTML, sin librerías ni assets externos (salvo fuentes).

**Jugar:** https://jesus1942.github.io/fortaleza-roja/

## Qué tiene

- Mundo **infinito procedural**: laberinto por bloques de 16×16 (4×4 celdas con pasillos
  de 3 de ancho), árbol de expansión + lazos para que no haya callejones sin salida, halls
  de 7×7 y puertas garantizadas entre bloques: siempre queda todo conectado. Cuanto más te
  alejás del origen, más dura la zona (los primeros bloques y los primeros 20 s son tranquilos).
- Texturas, criaturas, armas y objetos **dibujados por código** (diseño original).
- Tres tipos de enemigos con IA propia (persecución, distancia, cuerpo a cuerpo y proyectiles).
- Pistola y escopeta, botiquines, munición y cristales de puntos.
- Sonido **sintetizado** con Web Audio (sin archivos de audio).
- Controles de teclado + mouse (pointer lock) y **controles táctiles** con joystick virtual.
- Récord guardado en `localStorage`.

## Controles

| Entrada | Acción |
|---|---|
| W A S D / flechas | moverse (adelante, atrás, de costado) |
| Mouse / touchpad | girar y mirar, a la vez que caminás (clic para capturarlo; sin captura, arrastrar con botón derecho) |
| Q · E | girar con teclado |
| R · F · C | mirar arriba · abajo · centrar |
| Clic / Espacio | disparar |
| 1 · 2 | pistola · escopeta |
| Shift | correr |
| M | mapa |
| Gamepad | stick izq. moverse · stick der. girar · RT/A disparar · Y/LB/RB cambiar arma · Start pausa |
| Táctil | joystick a la izquierda, deslizar a la derecha para girar, botones FUEGO y ARMA |

La sensibilidad del mouse se ajusta en el menú y en la pausa, y queda guardada.

> **Touchpad:** muchos sistemas apagan el touchpad mientras se aprieta una tecla
> («deshabilitar touchpad al escribir»). Si no gira mientras caminás, desactivá esa opción.

## Correr localmente

Abrí `index.html` en el navegador. No necesita build.

---

Jesús Olguín · Domotics & IoT Solutions
