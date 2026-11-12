# VIRTUAL AIR JAM - LIVE CONCERT

## 1. Idea General del Proyecto
**Virtual Air Jam - Live Concert** es una experiencia de concierto virtual interactiva desarrollada para navegadores web. Permite a los usuarios convertirse en músicos en vivo utilizando únicamente sus movimientos corporales y gestos capturados en tiempo real mediante la cámara web y visión artificial con **ML5.js (PoseNet)**, combinando gráficos 3D/2D en **p5.js** y audio sintetizado/local mediante la **Web Audio API**.

El usuario puede alternar dinámicamente entre cuatro instrumentos mediante botones flotantes interactivos en las esquinas de la pantalla:
1. **Guitarra Superstrat:** Modelo detallado con mástil, diapasón, 17 trastes, puente Floyd Rose, humbuckers y manos cibernéticas animadas que se desplazan y rasguean al detectar velocidad de movimiento (`strumSpeed > 18`).
2. **Batería Interactiva:** Kit estático centrado con bombo, 4 toms mapeados de izquierda a derecha (Snare, Tom 1, Tom 2, Tom 3), platillos y baquetas virtuales con puntas brillantes y manos VR orientadas correctamente.
3. **Trompeta Dorada:** Instrumento con pivote en las manos y sensor de proximidad a la boca/cara (`distMouth < 75`) que activa audio continuo, ondas sonoras expansivas y notas musicales flotantes (`♪`, `♫`, `♩`).
4. **Teclado 3D Laser:** Sintetizador polifónico analógico estilo 80s (Doble Oscilador: Sawtooth + Square) con escala pentatónica y manos translúcidas moradas con 5 dedos extendidos.

---

## 2. Stack Tecnológico
- **Frontend / Gráficos:** HTML5, CSS3, JavaScript plano (ES6+).
- **Librería de Gráficos y Renderizado:** p5.js (v1.9.0) en modo interactivo sobre canvas de 640x480.
- **Inteligencia Artificial y Visión por Computador:** ml5.js (v0.12.2) utilizando el modelo **PoseNet** para detección de esqueletos (muñecas, nariz, rodillas, cadera).
- **Audio y Síntesis:** Web Audio API (Osciladores, Filtros Biquad, Ganancias y Envolventes ADSR) y soporte para archivos locales (`.mp3` y `.wav`) con clonación polifónica y control de reproducción continua.

---

## 3. Evolución y Decisiones de Diseño Clave
- **Cámara y Atmósfera:** Opacidad equilibrada (`fill(7, 10, 18, 110)`) sobre la cámara espejada para lograr una estética de concierto en vivo con luces de escenario dinámicas y ecualizador reactivo al movimiento (*Stage Hype*).
- **Pivote Dinámico de la Trompeta:** Inicialmente el pivote estaba fijo en la boca, lo que dejaba el instrumento flotando extrañamente al bajar los brazos. Se rediseñó para que el ancla principal sea en las manos del usuario y el extremo posterior verifique la distancia a la cara.
- **Orientación de Manos VR:** Inversión vertical (`scale(1, -1)`) en la guitarra para alinear correctamente la palma sosteniendo el mástil por debajo y la púa rasgueando las 6 cuerdas neón.
- **Mapeo Lógico de Batería:** Distribución de izquierda a derecha con muestras independientes asignadas a Snare, Tom1, Tom2 y Tom3.

---

## 4. Retos y Errores Principales Superados

| Error / Reto Detectado | Causa Raíz | Solución Implementada |
| :--- | :--- | :--- |
| **`ReferenceError: THREE_PI_2 is not defined`** | Uso de una constante de ángulo no estándar en p5.js al renderizar arcos en la trompeta. | Sustitución estricta por `1.5 * PI`. |
| **Guitarra acumulando sonido (Eco y persistencia)** | Uso de `.cloneNode()` en bucle continuo sobre un audio largo `.wav` al rasguear, impidiendo que se apague al cambiar de instrumento. | Implementación de un temporizador de rasgueo (`guitarActiveTimer`) con instancia única y parada/reseteo forzoso (`pause()` y `currentTime = 0`) al salir del modo guitarra. |
| **Inversión de dedos en el mástil** | La escala aplicada invertía la posición vertical del agarre, haciendo que los dedos apuntaran hacia abajo. | Corrección de la matriz de escala (`scale(1, 1)`) y redibujo de los nodos digitales orientados hacia arriba abrazando el diapasón. |
| **Bloqueo de Audio por CORS** | Carga de archivos locales `file:///` bloqueada por políticas de seguridad de los navegadores modernos. | Uso obligatorio de un servidor local de desarrollo como **Live Server** en Visual Studio Code (`http://127.0.0.1:5500`). |

---

## 5. Instrucciones de Instalación y Ejecución local
1. Clona este repositorio o descarga el proyecto completo en tu computador.
2. Asegúrate de tener una carpeta llamada exactamente **`audio`** en la raíz del proyecto conteniendo los archivos requeridos:
   - `Guitar.wav`
   - `Trumpet.mp3`
   - `Kick.wav`
   - `Snare.wav`
   - `Tom1.wav`, `Tom2.wav`, `Tom3.wav`
   - `Cymbal1.wav`, `Cymbal2.wav`
3. Abre la carpeta del proyecto en **Visual Studio Code**.
4. Instala y abre la extensión **Live Server**.
5. Haz clic derecho sobre el archivo `index.html` y selecciona **"Open with Live Server"**. Permite el acceso a la cámara web cuando el navegador lo solicite.
