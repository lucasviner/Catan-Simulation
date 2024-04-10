# Ciutats i Camins

Este proyecto consiste en una simulación de un juego inspirado en Catán, implementado en Python. Está diseñado para visualizar partidas descritas por archivos de texto, donde los jugadores pueden recolectar recursos, construir ciudades y crear caminos.

## Contenido del Proyecto

Dentro de la carpeta `skeleton`, encontrarás los siguientes componentes esenciales para el funcionamiento del juego:

- **7 scripts de Python**: Estos scripts coordinan el desarrollo del juego, separando los objetos individuales, la coordinación de los turnos y la interfaz visual.
- **Logo de "Ciutats i Camins"**: El logo visual del proyecto.
- **3 casos de prueba**: Archivos de entrada que permiten probar diferentes escenarios del juego y asegurar su correcto funcionamiento.
- **README.md**: Este archivo que estás leyendo, el cual provee una visión general del proyecto, instrucciones de uso y otra información relevante.
- **screenshot.png** Una captura de pantalla del juego.

### Modificaciones y Características

En la siguiente imagen, se muestra una captura de pantalla que ilustra las modificaciones realizadas sobre el script `drawer.py`. Estas modificaciones permiten visualizar en pantalla el turno actual, el saldo de cada jugador, y el ganador o estado de empate al final de la partida.

![Captura de pantalla del juego](screenshot.png)

Al realizar las modificaciones en `drawer.py` y completar los diferentes scripts, me he esforzado por respetar la estructura del código original. He seguido todas las normas establecidas en la documentación de GitHub y he añadido comentarios breves que facilitan la comprensión de la estructura y lógica del código.

### Cómo Ejecutar el Juego

Para ejecutar el juego, abre la carpeta `skeleton` desde una terminal y ejecuta los siguientes comandos uno por uno. Cuando se abra la pantalla del juego, pulsa la tecla "enter" para avanzar de turno.

```bash
python3 play.py < sample.inp
```
```bash
python3 play.py < test1.inp
```
```bash
python3 play.py < test2.inp
```
