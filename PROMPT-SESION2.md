# PROMPT SESIÓN 2 — Alex y Alexandra: El Tesoro de las Teclas II

> **Objetivo:** Tomar el juego existente `index.html` (un juego de escritura en teclado para dos jugadores) y mejorarlo para hacerlo **super chingón**: más visual, más recompensas, más contenido, mejor feedback, y más divertido. El juego ya funciona — hay que pulirlo, no reescribirlo desde cero.

---

## 1. Contexto del proyecto

- **Archivo base:** `/Users/navirami/Desktop/alex-sesion-2/index.html` (760 líneas)
- **Archivo a crear:** Copia del original mejorada (mismo nombre o `index-v2.html`)
- **Tipo:** Juego de escritura en teclado para 2 jugadores (turnos alternados)
- **Público:** Niños de 2°–3° de primaria (7–9 años) que ya saben leer y escribir
- **Tecnología:** HTML5 + CSS3 + JS vanilla, un solo archivo, sin dependencias

---

## 2. Qué tiene el juego actual (no romper)

| Elemento | Estado actual |
|---|---|
| 2 jugadores (Alexandra/Alex) | ✅ Funciona — turnos en misión 5 |
| 10 misiones con escenas | ✅ Funciona — forest, water, cave, box, gold, dragon, mar, montana, castillo, casa |
| Oraciones en español | ✅ Funciona — con acentos (á,é,í,ó,ú) y signos (¡,¿) |
| Teclado virtual con luces | ✅ Funciona — resalta tecla correcta, muestra Shift/Caps |
| Sistema de monedas 🪙 | ✅ Funciona — +10 por misión, +5 cada 10 racha, +20 por cofre |
| Sistema de racha 🔥 | ✅ Funciona — contador de aciertos seguidos |
| 5 cofres/tesoros 🧰 | ✅ Funciona — se abren cada 2 misiones |
| Speech synthesis | ✅ Funciona — lee oraciones y palabras |
| Personajes con sprites | ⚠️ Parcial — carga PNGs de assets/, fallback a emoji |
| Confeti | ✅ Funciona |
| Pista (hint) | ✅ Funciona — aparece en 3 segundos |

---

## 3. Mejoras a implementar (el "súper chingón")

### 3.1 VISUAL — Hacer que se vea increíble

**a) Escenarios con parallax scrolling:**
- Cada escena tiene 3 capas: cielo lejano, elementos medios, piso
- Al mover el personaje, las capas se mueven a diferente velocidad
- Efecto de profundidad que hace que el juego se sienta "vivo"

**b) Personajes SVG vectoriales:**
- Reemplazar emoji por SVGs dibujados con código (como el juego de la granja)
- Alexandra: cabello largo, vestido, expresión alegre
- Alex: cabello corto, overol, expresión alegre
- Animaciones: saltar al acertar, celebrar al completar misión
- Que se vean como personajes de caricatura, no como texto

**c) Teclado visual mejorado:**
- Teclas con sombra 3D realista
- Animación de "presionar" al escribir (tecla baja visualmente)
- Glow effect en la tecla correcta (resplandor dorado pulsante)
- Tecla correcta más grande que las demás
- Colores por fila: fila números = azul, fila QWERTY = verde, fila ASDF = naranja, fila ZXCV = morado

**d) Barra de progreso animada:**
- Barra con gradientes que cambian de color al avanzar
- Efecto de "brillo" en el borde cuando avanza
- Icono del personaje caminando sobre la barra

**e) Partículas y efectos:**
- Monedas que vuelan hacia el contador al completar misión
- Estrellas al acertar letra difícil (acentos, ñ, signos)
- Confeti con más colores y formas (círculos, estrellas, cuadrados)
- Brillo en los tesoros al abrir cofre

### 3.2 GAMEPLAY — Hacer que se sienta más divertido

**a) Combo system:**
- Cada 5 aciertos seguidos: ×1.5 puntos
- Cada 10 aciertos seguidos: ×2 puntos + monedas extra
- Cada 15 aciertos seguidos: ×3 puntos + confeti especial
- Si falla: combo se pierde, pero puntos no bajan (solo se pierde el multiplicador)

**b) Letras especiales con recompensa extra:**
- Tecla acentuada (á,é,í,ó,ú): +3 monedas extra
- Tecla ñ: +3 monedas extra
- Signos (¡,¿): +2 monedas extra
- Espacio: +1 moneda extra
- Feedback visual especial al acertar estas teclas

**c) Modo rápido (speed round):**
- Después de cada 3 misiones: 15 segundos para escribir tantas letras como pueda
- Cada letra = +1 moneda
- Reloj visible con countdown
- Música acelerada

**d) Power-ups comprables en tienda:**
| Power-up | Costo | Efecto |
|---|---|---|
| 💡 Pista automática | 30 🪙 | Muestra la tecla correcta inmediatamente (sin esperar 3s) |
| ⏰ Tiempo extra | 50 🪙 | En modo rápido, agrega 10 segundos |
| 🛡️ Escudo | 40 🪙 | Protege contra 1 error (no pierde racha) |
| ✨ Doble monedas | 60 🪙 | Por 3 misiones, gana el doble de monedas |
| 🎵 Música | 20 🪙 | Activa música de fondo sintetizada |

**e) Niveles de dificultad:**
- **Fácil:** Letras grandes, pista en 2 segundos, sin acentos
- **Normal:** Como está ahora (pista en 3 segundos)
- **Difícil:** Sin pista, oraciones más largas, tiempo limitado

### 3.3 CONTENIDO — Más y mejor

**a) Oraciones nuevas (agregar, no reemplazar):**
Mantener las 10 actuales y agregar 10 más variadas:
- Oraciones con números: "Veo 3 pájaros en el árbol."
- Oraciones con emociones: "¡Estoy muy feliz hoy!"
- Oraciones con preguntas: "¿Cuántos años tienes?"
- Oraciones con exclamaciones: "¡Qué día tan bonito!"
- Oraciones con ñ: "El niño tiene un señor."
- Oraciones mixtas: "Alex y Alexandra fueron a la playa."

**b) Escenas nuevas:**
- 🏖️ Playa
- 🌋 Volcán
- 🎪 Circo
- 🚀 Espacio
- 🏰 Castillo medieval

**c) Tesoros nuevos (expandir de 5 a 10):**
| # | Tesoro | Icono | Descripción |
|---|---|---|---|
| 1 | Brújula exploradora | 🧭 | "Siempre apunta al tesoro" |
| 2 | Llave de la cueva | 🔑 | "Abre puertas secretas" |
| 3 | Dragón amigo | 🐲 | "Guardián de las letras" |
| 4 | Gema de la montaña | 💎 | "Brilla con luz propia" |
| 5 | Corona del teclado | 👑 | "Corona de la escritura" |
| 6 | Mapa del tesoro | 🗺️ | "Muestra caminos ocultos" |
| 7 | Amuleto de la suerte | 🧿 | "Protege contra errores" |
| 8 | Libro de hechizos | 📖 | "Contiene palabras mágicas" |
| 9 | Estrella fugaz | ⭐ | "Cumple deseos" |
| 10 | Cofre dorado | 🏆 | "El tesoro final" |

### 3.4 AUDIO — Sonidos que enamoran

**a) Web Audio API (sintetizado, sin archivos):**
| Evento | Sonido |
|---|---|
| Letra correcta | Tono agudo corto (880 Hz, 0.08s) |
| Letra incorrecta | Tono grave corto (220 Hz, 0.12s, triangle) |
| Combo ×1.5 | Acorde mayor suave |
| Combo ×2 | Acorde mayor + octava |
| Combo ×3 | Fanfarria completa |
| Cofre abriéndose | Tones ascendentes (440→880→1320 Hz) |
| Monedas cayendo | Tones aleatorios agudos (rapido) |
| Misión completa | Fanfarria + aplausos |
| Power-up comprado | "Ka-ching" |
| Música de fondo | Loop suave de 4 acordes (I-V-vi-IV) |

**b) Speech synthesis mejorado:**
- Velocidad más lenta para oraciones largas
- Voz diferente para Alexandra (más aguda) y Alex (más grave)
- Decir "¡Muy bien!" al completar misión

### 3.5 TIENDA — Donde gastar monedas

**Acceso:** Botón "🛒 Tienda" visible en el HUD (siempre)

**Categorías:**
1. **Power-ups** (ver sección 3.2d)
2. **Decoraciones para la granja:**
   - 🌻 Girasoles (10 🪙)
   - 🦋 Mariposas (15 🪙)
   - 🌈 Arcoíris (25 🪙)
   - 🏠 Casa del árbol (40 🪙)
   - 🎈 Globos (20 🪙)
3. **Skins de personaje:**
   - 👒 Sombrero de granjero (30 🪙)
   - 🧣 Bufanda (25 🪙)
   - 👓 Lentes (35 🪙)
   - 🎩 Copa (45 🪙)

**Persistencia:** Todo guardado en localStorage:
- `alex2_coins`: monedas totales
- `alex2_purchased`: array de items comprados
- `alex2_level`: nivel alcanzado
- `alex2_bestStreak`: mejor racha
- `alex2_tesoros`: tesoros desbloqueados

### 3.6 MENÚ PRINCIPAL — Primera impresión

**Reemplazar el overlay de inicio por un menú completo:**

```
┌─────────────────────────────────────────┐
│                                         │
│     🧒 Alex y Alexandra 👧             │
│     El Tesoro de las Teclas II          │
│                                         │
│     ┌─────────┐  ┌─────────┐           │
│     │  FÁCIL  │  │ NORMAL  │           │
│     │  🌱     │  │  ⭐     │           │
│     └─────────┘  └─────────┘           │
│     ┌─────────┐                        │
│     │DIFÍCIL  │                        │
│     │  🔥     │                        │
│     └─────────┘                        │
│                                         │
│     Jugador 1: [Alexandra    ]          │
│     Jugador 2: [Alex         ]          │
│                                         │
│     ▶ ¡A jugar!                        │
│                                         │
│     🏆 Récord: 0  🪙 Monedas: 0        │
│                                         │
└─────────────────────────────────────────┘
```

### 3.7 PANTALLA DE VICTORIA FINAL

**Al completar las 10 misiones:**

```
┌─────────────────────────────────────────┐
│                                         │
│           🏆 ¡VICTORIA! 🏆             │
│                                         │
│     Alexandra y Alex encontraron        │
│     el tesoro de las teclas             │
│                                         │
│     ┌───────────────────────┐           │
│     │ 🧭 🔑 🐲 💎 👑       │           │
│     │ 🗺️ 🧿 📖 ⭐ 🏆       │           │
│     │  10 tesoros encontrados│           │
│     └───────────────────────┘           │
│                                         │
│     🪙 350 monedas                     │
│     🔥 Mejor racha: 23                 │
│     🎯 Precisión: 94%                  │
│     ⏱ Tiempo: 12:34                    │
│                                         │
│     [🖨️ Imprimir medalla]              │
│     [🛒 Ir a la tienda]                │
│     [▶ Jugar otra vez]                 │
│                                         │
└─────────────────────────────────────────┘
```

---

## 4. Estructura del código (mejorar, no reescribir)

### 4.1 Mantener la arquitectura actual:
- `GAME.pages[]` → expandir de 10 a 20 misiones
- `players[]` → mantener con SVGs
- `el{}` → agregar nuevos elementos
- `onKey()` → mejorar con combo system
- `renderSentence()` → mejorar con animaciones
- `buildKb()` → mejorar visual

### 4.2 Agregar nuevos módulos:
```javascript
// Nuevo: sistema de tienda
const Shop = {
  items: [...],
  buy(id) { ... },
  render() { ... }
};

// Nuevo: sistema de combo
const Combo = {
  count: 0,
  multiplier: 1,
  add() { ... },
  reset() { ... }
};

// Nuevo: sistema de power-ups
const PowerUps = {
  active: [],
  use(id) { ... },
  tick() { ... }
};

// Nuevo: audio sintetizado
const Audio = {
  ctx: null,
  init() { ... },
  play(kind) { ... }
};

// Nuevo: partículas
const Particles = {
  pool: [...],
  emit(x, y, type) { ... },
  update(dt) { ... }
};
```

### 4.3 Estilo CSS mejorado:
- Variables CSS para colores fáciles de cambiar
- Animaciones más suaves (ease-out en vez de ease)
- Sombras más realistas
- Bordes redondeados más grandes
- Transiciones en todo

---

## 5. Reglas estrictas

1. **Un solo archivo** — todo inline (HTML + CSS + JS)
2. **Sin dependencias** externas
3. **No romper** funcionalidad existente
4. **Mejorar** visual y gameplay
5. **Guardar** progreso en localStorage
6. **Funcionar** en Chrome, Firefox, Edge
7. **Máximo ~300 nodos DOM** (performance)
8. **Sin comentarios** en el código final
9. **Funcionar** con `prefers-reduced-motion`
10. **Accessible** (aria-labels, contraste, focus-visible)

---

## 6. Orden de implementación

1. **Primero:** Copiar el archivo base
2. **Segundo:** Mejorar CSS (colores, sombras, animaciones)
3. **Tercero:** Agregar audio sintetizado (Web Audio API)
4. **Cuarto:** Agregar combo system
5. **Quinto:** Agregar tienda y power-ups
6. **Sexto:** Agregar más oraciones y escenas
7. **Séptimo:** Mejorar personajes con SVGs
8. **Octavo:** Agregar parallax scrolling
9. **Noveno:** Pantalla de victoria final
10. **Décimo:** Pulir y probar

---

## 7. Resumen de mejoras vs juego actual

| Aspecto | Actual | Mejorado |
|---|---|---|
| Escenas | 10 | 15 |
| Oraciones | 10 | 20 |
| Tesoros | 5 | 10 |
| Tienda | No | Sí (power-ups + decoraciones + skins) |
| Combo | No | Sí (×1.5, ×2, ×3) |
| Audio | Solo speech | Speech + efectos + música |
| Personajes | Emoji/PNG | SVG vectorial animado |
| Visual | Básico | Parallax + partículas + sombras 3D |
| Dificultad | 1 nivel | 3 niveles (fácil/normal/difícil) |
| Power-ups | No | 5 tipos |
| Velocidad | Normal | + modo rápido |
