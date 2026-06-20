# Aprender Jugando 🏝️

**Aprender Jugando** es una aplicación web educativa, autocontenida en un solo
archivo (`index.html`), con mecánica de juego estilo Minecraft/Catan. La premisa
es simple: **lo que aprendes es lo que te permite avanzar en el juego**.

Exploras una isla con un personaje que mueves con el teclado, descubres a los
aldeanos con los que debes hablar, y cada palabra que aprendes se convierte en tu
**moneda** (para comprar equipo) y en tu **arma** (para combatir y vencer al jefe).

El enfoque inicial es **aprender inglés** con vocabulario acotado a propósito,
para mantener el contenido seguro y enfocado.

---

## 🎮 Cómo se juega

| Acción | Teclado | Táctil |
|---|---|---|
| Moverse | Flechas o `WASD` | D-pad en pantalla |
| Interactuar | `E` | Botón dorado "Acércate a alguien" |
| Viajar entre islas | `M` | Botón 🚢 en "Mis palabras" |
| Cerrar diálogo | `Esc` | Botón del cuadro |

1. **Mueve a tu personaje** libremente por la isla y acércate a los personajes.
   El que tengas al lado aparece resaltado con un anillo dorado.
2. **Habla con los aldeanos (💬).** No te dan la respuesta: te dan una **pista
   socrática** y tú deduces el significado en un cuadro de diálogo de opción
   múltiple. Acertar = aprendes la palabra y ganas 💎.
3. **Repasa (🔁).** Con el tiempo algunas palabras se "oxidan". El guía de repaso
   te las vuelve a preguntar (repaso espaciado ligero) para afianzar el dominio.
4. **Compra equipo en la tienda (🏪).** Aquí no se paga solo con gemas:
   **demuestras lo aprendido** respondiendo qué significa una palabra. Si
   aciertas, el trato es tuyo. Hay **6 artículos**: la **espada** (necesaria
   para enfrentar al jefe) y 5 piezas de armadura que reducen el daño —
   **pechera** (-6), **escudo** (-5), **casco** (-4), **pantalones** (-3) y
   **botas** (-2).
5. **Combate enemigos (👾).** Combate de **dificultad inversa**: a mayor nivel de
   isla aparecen **más enemigos pero con menos vida** (uno por respuesta
   correcta). Cada acierto derrota a un enemigo; fallar te hace daño.
6. **Vence al jefe (👹/🐲) completando palabras.** Necesitas una **espada** y
   haber aprendido al menos 3 palabras. El jefe te pide **5 palabras que TÚ
   aprendiste durante el juego** (las que enseñan los aldeanos de la isla), pero
   ahora **con letras faltantes**: completas los huecos tocando los **botones de
   letras** (o escribiéndolas en el teclado). La dificultad sube palabra a
   palabra: la 1ª oculta **1 letra**, la 2ª **2**, … hasta la 5ª con **5 letras**
   (sin pasar de la longitud de la palabra).
   Acertar las 5 = victoria; fallar resta vida que tu armadura amortigua (si
   caes, te recuperas y retomas sin perder progreso).
7. **Viaja a la siguiente isla (🚢).** Al vencer al jefe se **desbloquea la
   siguiente isla** (el siguiente módulo de aprendizaje). Tu progreso, palabras
   y equipo se conservan.

El progreso se guarda automáticamente en el navegador (`localStorage`). Puedes
reiniciarlo desde **📖 Mis palabras → ↺ Reiniciar**.

### 🧭 Siempre sabes qué hacer

Bajo el HUD hay una **franja de objetivo** que te guía paso a paso (*aprende N
palabras → consigue una espada → vence al jefe → viaja*) y muestra tu progreso
de palabras de la isla (`📖 3/8`). El botón **❓ Ayuda** reabre las
instrucciones y la leyenda de personajes cuando lo necesites. En los retos de
opción múltiple puedes responder con las teclas **1–4** y avanzar con **Enter**.

### 🏅 Logros (medallas)

Tus hitos se reconocen con **medallas**: aprender tu primera palabra, encadenar
aciertos, dominar palabras, equiparte, despejar zonas, vencer jefes y explorar
islas. Cada vez que desbloqueas una aparece una celebración, y puedes ver todas
(obtenidas y por conseguir) desde el contador **🏅** del HUD o en **📖 Mis
palabras → 🏅 Logros**. Son la recompensa que da motivos concretos para seguir
aprendiendo y volver a jugar.

### 👤 Perfil del jugador

Desde el botón **👤 Perfil** ves una **figura de cuerpo completo** que va
**vistiendo el equipo que consigues** (casco, pechera, pantalones, botas, escudo
y espada): las piezas obtenidas aparecen resaltadas y las que faltan, atenuadas.
El perfil también resume tus gemas, palabras, logros y equipo, y permite
**editar tu nombre** con el botón ✏️.

---

## 🧩 De dónde sale

Este juego **combina dos prototipos** en una sola experiencia:

- La **exploración libre con movimiento** de un personaje por un mapa de tiles y
  los **cuadros de diálogo** modales (la versión más completa de las ventanas de
  diálogo).
- La **progresión por etapas estilo Catan**, el **combate de enemigos que se
  multiplican**, la **armadura** y los **jefes** del prototipo basado en escenas.

El resultado mantiene la dinámica de moverte con las flechas y descubrir a quién
hablar, pero con diálogos ricos y una estructura pensada para crecer.

---

## 🗺️ Arquitectura: cada isla es un módulo de aprendizaje

Todo el contenido es **data-driven**. El motor (movimiento, diálogos, tienda,
combate, jefe, viaje) es genérico y lee de un único array: `ISLANDS`.

Cada isla = un módulo independiente con su tema, mapa, vocabulario, aldeanos,
tienda y jefe. Hoy vienen incluidas:

1. **Isla Niebla** — inglés: saludos y básicos.
2. **Isla del Bosque** — inglés: naturaleza.

### ➕ Añadir una nueva isla (nuevo módulo)

Abre `index.html`, busca el array `ISLANDS` y añade un objeto nuevo. No hace
falta tocar el motor:

```js
{
  id:'mar',                      // identificador único de la isla
  name:'Isla del Mar',
  theme:'Inglés · el océano',
  floor:'G',                     // tile base (decorativo)

  // 1) VOCABULARIO del módulo. Cada 'id' debe ser único en TODO el juego.
  //    La 'hint' es una PISTA socrática, nunca la traducción directa.
  vocab:[
    {id:'fish', en:'fish', es:'pez',   hint:'Animal que vive y nada en el agua.'},
    {id:'boat', en:'boat', es:'barco', hint:'Lo usas para viajar sobre el agua.'},
    // ...al menos 3 palabras para poder vencer al jefe
  ],

  // 2) MAPA: filas de igual longitud. Tiles:
  //    W=agua (no caminable) · G=césped · P=camino · S=arena · N=nieve
  map:[
    "WWWWWWWWWWWWW",
    "WGGGGGGGGGGGW",
    "WGGGGGGGGGGGW",
    "WWWWWWWWWWWWW"
  ],
  spawn:{x:3,y:2},               // casilla caminable donde aparece el jugador

  // 3) OBJETOS / personajes (coordenadas x,y en el mapa):
  objects:[
    // teacher: enseña las palabras de 'words' (ids del vocab) con pistas
    {id:'teacherE', kind:'teacher', face:'🧑‍🏫', x:4, y:1, name:'Capitán Sol',
     words:['fish','boat']},
    // reviewer: repaso espaciado de palabras "oxidadas"
    {id:'reviewer', kind:'reviewer', face:'🐬', x:6, y:2, name:'Delfín Coco'},
    // shop: tienda de equipo (paga demostrando lo aprendido)
    {id:'shop',     kind:'shop',     face:'🏪', x:2, y:1, name:'Tienda del Puerto'},
    // enemies: combate de enemigos que se multiplican
    {id:'enemies',  kind:'enemies',  face:'🦀', x:8, y:1, name:'Banco de cangrejos'},
    // boss: jefe final; al vencerlo se desbloquea la siguiente isla del array
    {id:'boss',     kind:'boss',     face:'🦈', x:6, y:1, name:'Kraken', face2:'🏳️',
     greeting:'¡Nadie cruza mi mar sin demostrar lo que sabe!'}
  ]
}
```

Reglas rápidas:

- **`id` de cada palabra debe ser único en todo el juego** (el banco de palabras
  y el repaso son globales entre islas).
- Cada isla debería tener al menos **un `teacher`, una `shop` y un `boss`**;
  `reviewer` y `enemies` son recomendables pero opcionales.
- El **orden del array es la progresión**: vencer al jefe de la isla *N*
  desbloquea la isla *N+1*.
- Las pistas (`hint`) deben **orientar sin resolver** (mecánica socrática).

Así, "sumar nuevas islas" = "sumar nuevos módulos de aprendizaje" sin escribir
lógica nueva. La idea es ir creciendo con más temas (familia, comida, números…)
e incluso otros dominios (lectura, etc.).

---

## 🚀 Ejecutar

No requiere build ni dependencias. Es un único archivo autocontenido:

- **Doble clic** en `index.html`, o
- Ábrelo en cualquier navegador moderno.

Opcionalmente, para servirlo en local:

```bash
python3 -m http.server 8000
# luego abre http://localhost:8000
```

---

## 🧠 Principios de diseño

- **Aprender deduciendo, no memorizando:** los aldeanos dan pistas; el jugador
  infiere el significado.
- **El conocimiento es el recurso:** las palabras son moneda (tienda) y arma
  (combate y jefe).
- **Refuerzo en el tiempo:** las palabras se "oxidan" y se repasan (SRS ligero).
- **Dificultad inversa:** más enemigos con menos vida a medida que avanzas.
- **Contenido acotado y extensible:** vocabulario seguro y enfocado, organizado
  en módulos (islas) fáciles de ampliar.
- **Recompensa y guía constantes:** una franja de objetivo dice siempre qué
  hacer, y los **logros** reconocen cada hito para sostener la motivación.

### ➕ Añadir un logro nuevo

Los logros también son **data-driven**: abre `index.html`, busca el array
`MEDALS` y añade un objeto con `id`, `ico`, `name`, `desc` y una condición
`cond(state, stats)` que devuelve `true` cuando se desbloquea. El motor lo
comprueba solo tras cada respuesta e hito.

```js
{ id:'words20', ico:'🌟', name:'Sabio', desc:'Aprende 20 palabras.',
  cond:(s)=>learnedCount()>=20 }
```

Helpers disponibles para las condiciones: `learnedCount()`, `masteredCount()`,
y el objeto `stats` (`bestStreak`, `correctStreak`, `totalCorrect`,
`zonesCleared`).

### 🐲 Las palabras del jefe son las que aprendes

La pelea del jefe **no usa un diccionario aparte**: pide exactamente las
**palabras que el jugador ha aprendido** con los aldeanos (el `vocab` de la
isla, ver `pickBossWords()` en `index.html`). Así, lo que estudias durante el
juego es justo lo que el jefe te pide completar. El motor:

- toma las palabras **aprendidas de la isla actual** (si faltan para llegar a 5,
  completa con cualquier otra aprendida y, si aún faltan, repite la más larga),
- las ordena por longitud para que el reto **escale** (más letras ocultas), y
- **oculta `n` letras en la palabra `n`** (1→5) sin pasar de su longitud.

Por tanto, para sumar vocabulario al jefe basta con **añadir palabras al `vocab`
de las islas** (mismo flujo que ya documenta "Añadir una nueva isla").

---

## 🗂️ Estructura del repositorio

```
.
├── index.html   # el juego completo (HTML + CSS + JS, sin dependencias)
└── README.md    # este archivo
```
