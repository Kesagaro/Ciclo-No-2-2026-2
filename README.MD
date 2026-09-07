# slotMachine — DOPO-POOB · Ciclos 1 y 2 · 2026-2

> Simulador de tragamonedas inspirado en el **Problem I – Slot Machine** de la ICPC World Finals 2025 (Baku).  
> En estos ciclos **no** se resuelve el problema de la maratón; solo se construye el simulador.

---

## Descripción

**slotMachine** es un simulador gráfico de máquina tragamonedas desarrollado en Java con BlueJ.  
La máquina administra `n` ruedas, cada una con una secuencia compartida de símbolos identificados por **colores CSS**. El _jackpot_ ocurre cuando todas las ruedas muestran el mismo símbolo; en ese caso la interfaz lo indica visualmente con un banner dorado.  
El simulador puede operar en **modo invisible**: la lógica sigue funcionando aunque no se muestre ninguna ventana.

---

## Autores

| Nombre | Correo |
|---|---|
| Kevin Garzón Romero | kevin.garzon-r@mail.escuelaing.edu.co |
| Daniel Blanco Salazar | daniel.blanco-s@mail.escuelaing.edu.co |

---

## Requisitos funcionales

### Ciclo 1

| # | Caso de uso | Detalle | Estado |
|---|---|---|---|
| 1 | Crear máquina tragamonedas | `SlotMachine()` | [x] |
| 2 | Gestionar ruedas | `addWheel`, `delWheel` | [x] |
| 3 | Gestionar símbolos | `addSymbol`, `delSymbol` | [x] |
| 4 | Girar ruedas | `placeSymbol`, `spin(wheel)`, `spin()` | [x] |
| 5 | Consultar símbolos | `symbols()`, `distinctSymbols()`, `configuration()` | [x] |
| 6 | Consultar jackpot | `isJackpot()` | [x] |
| 7 | Visibilidad | `makeVisible()`, `makeInvisible()` (modo invisible funcional) | [x] |
| 8 | Terminar simulador | `exit()` | [x] |

### Ciclo 2

| # | Caso de uso | Detalle | Estado |
|---|---|---|---|
| 9  | Intercambiar ruedas | `swap(wheel1, wheel2)` | [x] |
| 10 | Fijar / soltar rueda | `lock(wheel)`, `unlock(wheel)` | [x] |
| 11 | Rotar rueda N pasos | `spin(wheel, steps)` — animado si visible | [x] |
| 12 | Configurar máquina | `spin(String[] setSymbols)` | [x] |

---

## Tecnologías

| Tecnología / Componente | Uso en el proyecto |
|---|---|
| **Java SE** (BlueJ) | Lenguaje y entorno de desarrollo |
| **javax.swing** (`JFrame`, `JPanel`, `JOptionPane`) | Ventana gráfica y diálogos de error |
| **java.awt** (`Graphics2D`, `Color`, `Ellipse2D`) | Renderizado 2D de formas |
| **Paquete `shapes`** — `Canvas` | Extendido: colores CSS adicionales, redimensionado dinámico, cierre de ventana |
| **Paquete `shapes`** — `Circle` | Representación visual de símbolos (`Symbol`) |
| **Paquete `shapes`** — `Rectangle` | Marco de ruedas (`Wheel`) y vistas (`SlotMachineView`) |
| **Paquete `shapes`** — `Triangle` | Disponible en el paquete (no usado en lógica actual) |
| **Astah** | Diagramas de clases y de secuencia |
| **Claude Sonnet 4.6** | Asistente de IA utilizado para resolver dudas sobre el código |

---

## Estructura del proyecto

```
Ciclo1v2/
├── shapes/                    # Paquete base reutilizado (Canvas, Circle, Rectangle, Triangle)
└── slotMachine/               # Proyecto principal
    ├── SlotMachine.java           # Clase principal (API pública del simulador)
    ├── Wheel.java                 # Rueda: lista circular, bloqueo, giro animado
    ├── Symbol.java                # Símbolo representado como círculo de color CSS
    ├── SlotMachineView.java       # Vista: fondo y banner de jackpot
    ├── Canvas.java                # Canvas extendido (singleton gráfico)
    ├── Circle.java                # Forma reutilizada del paquete shapes
    ├── Rectangle.java             # Forma reutilizada del paquete shapes
    ├── Triangle.java              # Forma disponible del paquete shapes
    ├── SlotMachineC2Test.java     # Pruebas unitarias propias Ciclo 2
    ├── SlotMachineCC2Test.java    # Pruebas unitarias compartidas Ciclo 2
    └── package.bluej              # Configuración del proyecto BlueJ
```

---

## Cómo ejecutar

### Requisitos previos

- **Java SE 8** o superior instalado.
- **BlueJ 5.x** instalado.

### Pasos

1. Abrir BlueJ y seleccionar **Project → Open Project…**
2. Navegar hasta la carpeta `Ciclo1v2/slotMachine/` y abrirla.
3. Compilar todas las clases con **Tools → Compile**.
4. En el _Object Bench_, hacer clic derecho sobre `SlotMachine` → **new SlotMachine()**.
5. Llamar `makeVisible()` sobre el objeto para mostrar la ventana gráfica.
6. Interactuar con los métodos disponibles (`addWheel`, `addSymbol`, `spin`, `swap`, `lock`, etc.).

> **Nota:** El simulador también funciona en modo invisible (sin llamar `makeVisible()`).  
> Los errores solo se muestran con `JOptionPane` cuando el simulador está visible.

---

## Diseño

Los diagramas de diseño fueron elaborados con **Astah** y se encuentran en la carpeta `diseño/` del repositorio:

- **Diagrama de clases** — incluye atributos y métodos públicos y privados.  
  De las clases de `shapes`, se documenta únicamente lo extendido (`Canvas`).
- **Diagramas de secuencia** — uno por cada método público de `SlotMachine`.  
  Los diagramas paran en los componentes de `shapes` sin entrar en su detalle interno.

### Resumen de la arquitectura

```
SlotMachine
  ├── ArrayList<Wheel>          // n ruedas
  ├── ArrayList<String>         // secuencia compartida de colores CSS
  └── SlotMachineView           // fondo negro + banner jackpot (gold)
        └── Rectangle (×2)

Wheel
  ├── ArrayList<Symbol>         // símbolos en orden circular
  ├── int currentIndex          // símbolo visible
  ├── boolean locked            // estado de bloqueo (Ciclo 2)
  └── Rectangle                 // marco (azul=libre, rojo=bloqueado)

Symbol
  └── Circle                    // círculo de color CSS (diámetro 56 px)

Canvas  (extendido)
  └── JFrame + JPanel           // ventana con buffer offscreen
```

---

## Mini-ciclos

### Ciclo 1 (17 – 23 agosto)

| # | Fechas | Descripción | Criterio de completitud | Estado |
|---|---|---|---|---|
| 1 | 17 ago | **Análisis y planificación** — Lectura del enunciado, comprensión del paquete `shapes`, definición de arquitectura y reparto de tareas. | Árbol de clases definido. | [x] |
| 2 | 18–19 ago | **Clases del dominio** — `Symbol` (círculo de color CSS) y `Wheel` (lista circular con marco rectangular). | `Symbol` y `Wheel` funcionando en modo invisible. | [x] |
| 3 | 19–20 ago | **Clase principal** — `addWheel`, `delWheel`, `addSymbol`, `delSymbol`, clamping y `ok()`. | Métodos de gestión pasan pruebas manuales en BlueJ. | [x] |
| 4 | 20–21 ago | **Giro y consultas** — `spin(wheel)`, `spin()`, `placeSymbol()`, `symbols()`, `distinctSymbols()`, `configuration()` e `isJackpot()`. | Jackpot detectado correctamente y consultas coherentes. | [x] |
| 5 | 21–23 ago | **Capa visual y entrega** — `Canvas` extendido, `SlotMachineView`, visibilidad, `exit()`, diagramas Astah, publicación en Git. | Simulador visible con banner dorado; repositorio publicado. | [x] |

### Ciclo 2 (31 agosto – 6 septiembre)

| # | Fechas | Descripción | Criterio de completitud | Estado |
|---|---|---|---|---|
| 1 | 31 ago | **Análisis Ciclo 2** — Lectura del enunciado, actualización de diagramas Astah con los 5 métodos nuevos. | Diseño actualizado y tareas distribuidas. | [x] |
| 2 | 1 sep | **Bloqueo en `Wheel`** — Campo `locked`, métodos `lock()`, `unlock()`, `isLocked()` e indicador visual (marco rojo/azul). | Rueda bloqueada no gira; indicador visual correcto. | [x] |
| 3 | 2 sep | **`swap` en `SlotMachine`** — Intercambio de ruedas con reposicionamiento visual. | Configuración visual intercambia correctamente. | [x] |
| 4 | 3 sep | **`spin(steps)` y `spin(String[])`** — Giro animado por pasos y configuración directa de la máquina. | Animación visible paso a paso; configuración aplicada. | [x] |
| 5 | 4–5 sep | **Pruebas unitarias** — `SlotMachineC2Test` (12 casos propios) y `SlotMachineCC2Test` (2 casos compartidos), todos en modo invisible. | Todos los casos pasan sin errores. | [x] |
| 6 | 5–6 sep | **Integración y entrega** — Revisión de código, ajuste de comentarios, actualización del README, publicación en Git. | Repositorio actualizado antes de la fecha de entrega. | [x] |

---

## Retrospectiva

### Ciclo 1

1. **¿Cuáles fueron los mini-ciclos definidos? Justifíquenlos.**

   Se definieron 5 mini-ciclos organizados por capas de responsabilidad: análisis y planificación, clases del dominio (`Symbol`, `Wheel`), clase principal `SlotMachine`, lógica de giro y consultas, y capa visual con entrega. Esta división permitió avanzar de forma incremental, validando cada capa antes de construir sobre ella.

2. **¿Cuál es el estado actual del proyecto en términos de mini-ciclos? ¿Por qué?**

   Los 5 mini-ciclos del Ciclo 1 están completados. Todos los requisitos funcionales fueron implementados dentro del plazo previsto (23 de agosto).

3. **¿Cuál fue el tiempo total invertido por cada uno de ustedes? (Horas/Hombre)**

   | Integrante | Horas |
   |---|---|
   | Kevin Garzón Romero | ~15 h |
   | Daniel Blanco Salazar | ~15 h |
   | **Total** | **~30 h** |

4. **¿Cuál consideran fue el mayor logro? ¿Por qué?**

   Lograr que el simulador funcione en modo visible e invisible, respetando la API definida en el enunciado, fue el mayor logro. Implicó coordinar el estado interno de las ruedas con la capa gráfica del `Canvas` extendido sin romper la lógica de negocio.

5. **¿Cuál consideran que fue el mayor problema técnico? ¿Qué hicieron para resolverlo?**

   La falta de conocimiento sobre el manejo del `Canvas` con buffer offscreen, los colores CSS en Java y el posicionamiento de formas con coordenadas absolutas generó retrasos. Lo resolvimos consultando la documentación de Java, W3Schools y usando Claude Sonnet 4.6 para dudas puntuales.

6. **¿Qué hicieron bien como equipo? ¿Qué se comprometen a hacer para mejorar los resultados?**

   Leímos todo el documento de requisitos al inicio y separamos el trabajo en mini-ciclos para organizar mejor el tiempo. Como compromiso para el siguiente ciclo, mejorar la comunicación continua y escribir pruebas más sistemáticas.

7. **Considerando las prácticas XP incluidas en los laboratorios, ¿cuál fue la más útil? ¿Por qué?**

   Los laboratorios 1 y 2 nos dieron base sobre el manejo de objetos en Java y el uso del paquete `shapes`. Eso nos permitió tener más estructura en el código desde el inicio.

8. **¿Qué referencias usaron? ¿Cuál fue la más útil?**

   | # | Referencia | Uso |
   |---|---|---|
   | 1 | W3Schools. (s.f.). *CSS Legal Color Values*. https://www.w3schools.com/cssref/css_colors.php | Nombres de colores CSS para los símbolos. |
   | 2 | Oracle. (s.f.). *ArrayList (Java SE 8)*. https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html | Métodos de `ArrayList` en `Wheel` y `SlotMachine`. |
   | 3 | Oracle. (s.f.). *javax.swing.JOptionPane*. https://docs.oracle.com/javase/8/docs/api/javax/swing/JOptionPane.html | Diálogos de error en modo visible. |
   | 4 | Kolling, M. & Barnes, D. J. (2016). *Objects First with Java* (6.ª ed.). Pearson. | Referencia principal de diseño OO y uso de BlueJ. |
   | 5 | Anthropic. (2026). *Claude Sonnet 4.6* [IA]. https://www.anthropic.com | Resolución de dudas sobre el código. |

   La más útil fue **W3Schools (CSS Color Values)**.

---

### Ciclo 2

1. **¿Cuáles fueron los mini-ciclos definidos? Justifíquenlos.**

   Se definieron 6 mini-ciclos de un día cada uno: análisis y diseño, bloqueo en `Wheel`, `swap`, `spin(steps)` y `spin(String[])`, pruebas unitarias, e integración y entrega. Un mini-ciclo por día permitió mantener el foco en una sola responsabilidad sin mezclar cambios.

2. **¿Cuál es el estado actual del proyecto en términos de mini-ciclos? ¿Por qué?**

   Casi todos los mini-ciclos están completados. Los 5 métodos nuevos funcionan correctamente y las pruebas unitarias cubren los casos principales. El mini-ciclo de integración y entrega se cerró el 6 de septiembre.

3. **¿Cuál fue el tiempo total invertido por cada uno de ustedes? (Horas/Hombre)**

   | Integrante | Horas |
   |---|---|
   | Kevin Garzón Romero | ~18–20 h |
   | Daniel Blanco Salazar | ~18–20 h |
   | **Total** | **~36–40 h** |

4. **¿Cuál consideran fue el mayor logro? ¿Por qué?**

   La correcta integración del código nuevo con el del Ciclo 1 sin romper nada de lo ya implementado. Agregar `locked`, `swap` y los nuevos `spin` requirió entender bien el estado interno de `Wheel` y `SlotMachine` para no generar efectos secundarios inesperados.

5. **¿Cuál consideran que fue el mayor problema técnico? ¿Qué hicieron para resolverlo?**

   La falta de conocimiento sobre cómo estructurar pruebas unitarias en BlueJ (sin JUnit formal) fue el mayor obstáculo. Lo resolvimos revisando los laboratorios 1 y 2 de la materia como referencia y consultando documentación de BlueJ para entender cómo ejecutar métodos de prueba manualmente.

6. **¿Qué hicieron bien como equipo? ¿Qué se comprometen a hacer para mejorar los resultados?**

   La comunicación fue bastante buena durante el ciclo y aprovechamos la base de los laboratorios 1 y 2 para estructurar mejor el código. Para mejorar, nos comprometemos a aumentar la eficiencia en la distribución de tareas y a mantener una comunicación más constante durante el desarrollo.

7. **Considerando las prácticas XP incluidas en los laboratorios, ¿cuál fue la más útil? ¿Por qué?**

   Los laboratorios 1 y 2 siguieron siendo la referencia más útil. El laboratorio 2 en particular, por el uso del paquete `shapes` y el `Canvas`, fue clave para entender cómo integrar los cambios visuales del Ciclo 2 (indicador de bloqueo, animación de giro) sin alterar la lógica existente.

8. **¿Qué referencias usaron? ¿Cuál fue la más útil?**

   | # | Referencia | Uso |
   |---|---|---|
   | 1 | W3Schools. (s.f.). *CSS Legal Color Values*. https://www.w3schools.com/cssref/css_colors.php | Colores CSS para símbolos. |
   | 2 | Oracle. (s.f.). *ArrayList (Java SE 8)*. https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html | Métodos de colecciones en las clases del dominio. |
   | 3 | Oracle. (s.f.). *Thread.sleep / Canvas.wait*. https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html | Implementación del delay en `spin(steps)`. |
   | 4 | Kolling, M. & Barnes, D. J. (2016). *Objects First with Java* (6.ª ed.). Pearson. | Referencia de diseño OO, pruebas y BlueJ. |
   | 5 | Anthropic. (2026). *Claude Sonnet 4.6* [IA]. https://www.anthropic.com | Resolución de dudas sobre el código. |

   La más útil en este ciclo fue **Kolling & Barnes**, especialmente la sección de pruebas con BlueJ.
