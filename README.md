# Fortaleza Roja

FPS retro en primera persona hecho con **raycasting** sobre `<canvas>`, en un único
archivo HTML, sin librerías ni assets externos (salvo fuentes).

**Jugar:** https://jesus1942.github.io/fortaleza-roja/

## Qué tiene

- Mundo **infinito procedural** por bloques de 16×16, con puertas compartidas entre
  bloques para que siempre quede conectado. Cuanto más te alejás del origen, más dura la zona.
- Texturas, criaturas, armas y objetos **dibujados por código** (diseño original).
- Tres tipos de enemigos con IA propia (persecución, distancia, cuerpo a cuerpo y proyectiles).
- Pistola y escopeta, botiquines, munición y cristales de puntos.
- Sonido **sintetizado** con Web Audio (sin archivos de audio).
- Controles de teclado + mouse (pointer lock) y **controles táctiles** con joystick virtual.
- Récord guardado en `localStorage`.

## Controles

| Tecla | Acción |
|---|---|
| W A S D | moverse |
| Mouse / Q E / ← → | girar |
| R · F · C | mirar arriba · abajo · centrar |
| Clic / Espacio | disparar |
| 1 · 2 | pistola · escopeta |
| Shift | correr |
| M | mapa |

## Correr localmente

Abrí `index.html` en el navegador. No necesita build.

---

Jesús Olguín · Domotics & IoT Solutions
