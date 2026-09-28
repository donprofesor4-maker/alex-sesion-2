# SPE-2026-09-27 | Session 2 Enhancement Brief
## Project: Alex & Alexandra — Keyboard Treasure Hunt
### Platform: Web (Single-file HTML5) | Target: EDU K-3 | Build: Production

---

**FROM:** Senior Gameplay Engineer, EA Education Division
**TO:** Codex Build Agent (Astra 6)
**RE:** Session 2 Enhancement — Mouse Integration + Visual Overhaul
**CLASSIFICATION:** Production Build — Ship-Ready

---

## 0. SITUATION REPORT

We have a functional keyboard-typing game for 7-9 year old students. Two players alternate writing Spanish sentences. Virtual keyboard lights up correct key. Coins, streaks, treasure chests. Works. Ships. But it's 2026 and the game doesn't teach mouse skills — which is a critical gap for this age group. Kids this age can type but can't distinguish left-click from right-click. That's the problem we're solving.

**File to enhance:** `/Users/navirami/Desktop/alex-sesion-2/index.html`
**Lines:** ~760 | **Format:** Single HTML file, all inline, zero dependencies
**Status:** shippable baseline — DO NOT BREAK WHAT WORKS

---

## 1. PRIMARY OBJECTIVE: MOUSE MASTERY INTEGRATION

This is the #1 deliverable. Everything else is secondary.

### 1.1 Clickable Virtual Keyboard
The on-screen keyboard must become interactive via mouse:

- **Left-click** on key = VALID input for **vowels** (a, e, i, o, u, á, é, í, ó, ú) and **spacebar**
- **Right-click** on key = VALID input for **consonants** (b-z, ñ) and **special characters** (¡, ¿, punctuation)
- Each key displays a small badge: **"L"** (blue) for left-click or **"R"** (red) for right-click
- Wrong button pressed → key shakes, error tone plays, NO point penalty (combo breaks only)
- Correct button → same behavior as physical keypress (advances cursor, awards points)

### 1.2 Mouse Mode Toggle
New button in controls row: **"🖱️ Modo mouse"**

When toggled ON:
- Physical keyboard input is **disabled** (onKey handler returns immediately)
- Virtual keyboard keys scale up 15% and increase spacing
- A persistent banner appears above keyboard: **"Clic izquierdo = vocales · Clic derecho = consonantes"**
- Button glows red/pink when active with a breathing animation
- Mouse cursor changes to crosshair over keyboard area

### 1.3 Parallax Depth
Decorative elements in `.scene` (trees, clouds, flowers, owl) respond to mouse position:
- Calculate mouse offset from screen center
- Move each `.amb` element by `(offset * depth_factor)` where depth varies per element
- Max displacement: ~15px horizontal, ~8px vertical
- Disable when `prefers-reduced-motion: reduce`

---

## 2. AUDIO ENGINE — Web Audio API

Zero external files. All synthesized. Master gain node → destination.

### Sound Matrix

| Event | Oscillator | Freq | Duration | Type | Notes |
|-------|-----------|------|----------|------|-------|
| Correct key | sine | 880Hz + 1100Hz | 80ms + 60ms | sine | Two tones, 30ms offset |
| Wrong key | triangle | 220Hz + 180Hz | 150ms + 120ms | triangle | Low rumble |
| Combo ×1.5 | sine | 523-659-784Hz | 200ms each | sine | Major chord arpeggio |
| Combo ×2 | sine | 523-659-784-1046Hz | 300ms each | sine | + octave |
| Combo ×3 | sine | 523-659-784-1046-1318Hz | 400ms each | sine | Full fanfare |
| Chest open | sine | 440-554-659-880Hz | 400ms each | sine | Ascending |
| Coins | sine | 1200Hz + 1500Hz | 60ms + 50ms | sine | Quick sparkle |
| Mission complete | sine | 523-659-784-1046-1318Hz | 500ms each | sine | Full fanfare |
| Shop purchase | sine | 880Hz + 1320Hz | 80ms + 100ms | sine | Ka-ching |
| Mouse correct | sine | 880Hz + 1100Hz | 100ms + 80ms | sine | Confirm |
| Mouse wrong | triangle | 300Hz | 120ms | triangle | Reject |

**Ambient:** Optional — bandpass-filtered noise loop at 360Hz, gain 0.06. Birds via FM synthesis (random interval 4-9s).

**Master volume:** 0.2. Mute toggle persisted to localStorage key `alex2Muted`.

---

## 3. COMBO SYSTEM

```
combo_count = consecutive correct inputs (resets on wrong)
multiplier = 1 (base)
if combo_count >= 5:  multiplier = 1.5
if combo_count >= 10: multiplier = 2.0  + bonus 5 coins
if combo_count >= 15: multiplier = 3.0  + bonus 5 coins + special confetti
```

On wrong input:
- combo_count = 0, multiplier = 1
- Points NOT deducted (only multiplier resets)

Visual: Floating display "⚡ ×2" appears center-screen for 800ms with scale-in animation when multiplier changes.

---

## 4. SHOP SYSTEM

Button **"🛒 Tienda"** in controls row. Opens overlay with grid layout.

### Inventory

| ID | Name | Icon | Cost | Effect |
|----|------|------|------|--------|
| hint_auto | Pista auto | 💡 | 30 | Shows correct key immediately (skip 3s delay) |
| shield | Escudo | 🛡️ | 40 | Absorbs 1 error (combo preserved) |
| double_coins | Doble monedas | ✨ | 50 | 2× coins for next 3 missions |
| girasoles | Girasoles | 🌻 | 15 | Decoration |
| mariposas | Mariposas | 🦋 | 20 | Decoration |
| arcoiris | Arcoíris | 🌈 | 30 | Decoration |
| globos | Globos | 🎈 | 25 | Decoration |
| casa_arbol | Casa del árbol | 🏠 | 45 | Decoration |
| sombrero | Sombrero | 👒 | 35 | Skin |
| gafas | Lentes | 👓 | 40 | Skin |

**Purchase flow:** Click item → check coins ≥ cost → deduct coins → add to `purchased` Set → play shop sound → re-render grid → update HUD.

**Persistence:** `purchased` saved to localStorage key `alex2_purchased` as JSON array.

---

## 5. CONTENT EXPANSION

### 5.1 Missions: 10 → 20

Keep original 10. Add these after:

```javascript
{scene:"playa", sentence:"Las olas del mar son enormes.", flag:"🏖️"},
{scene:"volcan", sentence:"¡El volcán humea mucho!", flag:"🌋"},
{scene:"circo", sentence:"El payaso hizo 7 malabares.", flag:"🎪"},
{scene:"espacio", sentence:"Las estrellas brillan en la noche.", flag:"🚀"},
{scene:"medieval", sentence:"El caballero luchó con valentía.", flag:"⚔️"},
{scene:"forest", sentence:"Los pájaros cantan en el árbol.", flag:"🐦"},
{scene:"water", sentence:"El pez saltó fuera del agua.", flag:"🐟"},
{scene:"cave", sentence:"En la cueva había un tesoro dorado.", flag:"✨"},
{scene:"gold", sentence:"Alex y Alexandra encontraron todo.", flag:"🌟"},
{scene:"casa", sentence:"¡Aventura completa! Son los mejores.", flag:"🏆"}
```

Player switch stays at mission 10 (HALF = 10).

### 5.2 New Scene CSS Classes

```css
.scene.playa { background: linear-gradient(#87ceeb, #f4a460 72%); }
.scene.volcan { background: linear-gradient(#2f1b1b, #8b0000 72%); }
.scene.circo { background: linear-gradient(#ff6b6b, #ffa07a 72%); }
.scene.espacio { background: linear-gradient(#0a0a2e, #1a1a4e 72%); }
.scene.medieval { background: linear-gradient(#4a3728, #2a1f18 72%); }
```

Floor colors for each new scene must be defined.

### 5.3 Props

Expand PROPS_SHOW array from 10 to 20 entries. Use 4 prop icons: 🐸 🔑 💰 💎.

---

## 6. VISUAL OVERHAUL

### 6.1 CSS Variables (add to :root)

```css
:root {
  --gold: #ffd166; --gold-dark: #c99e2e;
  --orange: #f26d3d; --orange-dark: #b94c24;
  --green: #7bc85b; --green-dark: #4e9a33;
  --blue: #5da9ff; --purple: #c77bff;
  --red: #ff5d5d; --pink: #ff9ecb;
  --bg: #fff6e3; --text: #21425c; --text-light: #4a3208;
  --radius: 20px; --radius-sm: 12px;
}
```

### 6.2 Enhancements

- **Border radius:** All major containers → 20px
- **Shadows:** `0 8px 32px rgba(0,0,0,.18)` on game container
- **Glow:** `0 0 16px rgba(255,209,102,.5)` on lit keyboard keys
- **Transitions:** All interactive elements → `transition: all .2s ease`
- **HUD:** Horizontal bar with coin, streak, combo, chests, page label — subtle gradient background
- **Bar:** Gradient fill (green → blue) with inner shine pseudo-element
- **Buttons:** All buttons get `::after` gradient overlay for depth

### 6.3 Start Menu

Replace current overlay with:
- Character icons (👧🧒) row
- Title + subtitle
- **Difficulty selector:** 3 buttons (🌱 Fácil / ⭐ Normal / 🔥 Difícil)
  - Easy: skip accented chars, hint in 2s
  - Normal: current behavior, hint in 3s
  - Hard: no hints, longer sentences
- Name inputs for both players
- Start button

### 6.4 Victory Screen

After mission 20:
- Trophy emoji + congratulations
- Stats: coins earned, best streak, accuracy %, chests opened
- "🖨️ Imprimir medalla" button (triggers window.print())
- "🛒 Ir a la tienda" button
- "▶ Jugar otra vez" button
- Confetti burst (60 particles)

---

## 7. HUD STRUCTURE

```
┌─────────────────────────────────────────────────┐
│ 🪙 350  │ 🔥 12  │ ⚡ ×2  │ 🧰 3/5 │ Misión 7 │
└─────────────────────────────────────────────────┘
```

- Coins (gold color)
- Streak (red color)
- Combo (purple, hidden when < 3, pulses when visible)
- Chests (green)
- Page label

Combo display floats center-screen when multiplier changes.

---

## 8. PERSISTENCE

| Key | Type | Description |
|-----|------|-------------|
| `alex2_coins` | number | Total coins |
| `alex2_purchased` | JSON array | Bought item IDs |
| `alex2_bestStreak` | number | Best streak ever |
| `alex2_chests` | number | Chests opened |
| `alex2Muted` | "0"/"1" | Audio mute state |

Load on init. Save on: mission complete, purchase, game over.

---

## 9. TECHNICAL CONSTRAINTS — NON-NEGOTIABLE

1. **Single file** — everything inline in `index.html`
2. **Zero external dependencies** — no npm, no CDN, no imports
3. **Web Audio API** for all audio — no audio files
4. **requestAnimationFrame** for game loop
5. **Pointer Events API** for mouse/touch unification
6. **No comments** in final output
7. **Accessibility:** aria-labels on all interactive elements, aria-live for status messages, focus-visible outlines, 4.5:1 contrast ratio minimum
8. **Performance:** ≤400 DOM nodes target
9. **Reduced motion:** `prefers-reduced-motion: reduce` → disable all animations and confetti
10. **Browser compat:** Chrome, Firefox, Edge (latest 2 versions)
11. **Variable names in Spanish** where it improves readability
12. **DO NOT REMOVE** existing features — enhance only

---

## 10. DELIVERABLE

One file: `/Users/navirami/Desktop/alex-sesion-2/index.html`

Complete, working, production-ready. Opens in browser, runs with zero setup. All features functional. Ship it.

---

*End of brief. Execute.*
