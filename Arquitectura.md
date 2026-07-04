# Arquitectura - Timeline Tour de França 2026

## Estructura general

Un únic fitxer `index.html` que conté tot: HTML, CSS i JSX (React). No hi ha build step, no hi ha bundler, no hi ha servidor de backend.

```
index.html
├── <head>       → CDNs (Tailwind, Babel, React via importmap, lucide-react)
├── <style>      → Animacions CSS (spin, fadeIn, pulse-bar)
└── <script>     → Tot el codi React (JSX compilat per Babel al navegador)
```

---

## Font de dades

```
Google Drive (TIMELINE TOUR.xlsx)
  → Google Sheets (convertit automàticament)
    → Export CSV públic (fetch sense autenticació)
      → parseCSV() → groupByDay() → estat React
```

**URL del CSV:**
```
https://docs.google.com/spreadsheets/d/15O6ZoZjfTnzZKRDgGUZBNzuD88KyfvvJ/export?format=csv
```

---

## Estructura de columnes de l'Excel

| Índex | Columna | Contingut |
|-------|---------|-----------|
| r[0] | A | Data `DD/MM/YYYY` (capçalera de dia) o HORA (estructura antiga) |
| r[1] | B | Nom del dia `"Dilluns 22 de juny"` o HORA (estructura nova) |
| r[2] | C | HORA FI |
| r[3] | D | CONCEPTE (títol de la tasca) |
| r[4] | E | OBSERVACIONS (expandibles) |
| r[5] | F | UBICACIÓ |
| r[6] | G | RESPONSABLE |
| r[7] | H | ESTAT (`FET`, `EN PROCÉS`, `PENDENT`, o buit) |

> L'app detecta automàticament si una fila usa l'estructura antiga (HORA a col A) o nova (DATA a col A, HORA a col B).

---

## Components React

```
App
├── Header (logo, títol, comptador d'accions, dies actius)
│   └── Countdown (compte enrere fins al 5/7 a les 13:45h)
├── NavBar (pestanyes + mini-calendari de pills per saltar a cada dia)
├── CalendarView (vista principal)
│   ├── Filtres de responsable (pills globals)
│   └── per cada dia:
│       ├── Capçalera del dia
│       └── DayTimeline
│           └── EventCard (per cada tasca)
└── DayView (quan fas clic en un dia del mini-calendari)
    ├── Filtres de responsable (del dia)
    └── DayTimeline
        └── EventCard
```

### Components principals

**`App`** — Estat global: dades, dia seleccionat, filtres, vista activa. Fa el `fetch()` del CSV.

**`CalendarView`** — Mostra tots els dies. Separa futurs (en color) i passats (grisos al fons, etiquetats "Finalitzats"). Té filtre global de responsable.

**`DayTimeline`** — Llista de tasques d'un dia, agrupades per hora. Filtrables per responsable. Prop `isPast` per aplicar estils de dia passat.

**`EventCard`** — Una targeta de tasca. Expandible si té observacions. Aplica:
- Color de barra esquerra per responsable
- Estils de fons/opacitat per estat (FET/EN PROCÉS/PENDENT)
- Si `isPast` i l'estat no és `FET` ni buit: pill en color sobre grayscale (via overlay absolut)
- Línia de 3px a la part inferior per estat (animada per EN PROCÉS)

**`Countdown`** — Compte enrere fins al 5/7/2026 a les 13:45h.

---

## Parsing de dades

### `parseCSV(text)`
Parser CSV manual que gestiona cometes, comes dins camps i CRLF. No usa cap llibreria externa.

### `buildDays(rows)`
Construeix la llista ordenada de dies únics. Detecta capçaleres de dia per:
1. Data a columna A (`DD/MM/YYYY` o `YYYY-MM-DD`)
2. Fallback: text a columna B que comenci per nom de dia (`Dilluns`, `Lunes`, `Lundi`...)

### `groupByDay(rows)`
Agrupa les tasques per dia. Detecta automàticament l'estructura de cada fila (antiga o nova). Retorna un objecte `{ dayKey: [tasques] }`.

---

## Sistema de Responsables

```javascript
const RESP_MAP = [
  { match: 'ASO',               label: 'ASO',          dot: '#00b2e3' },
  { match: 'TGNA ESPORTS',      label: 'Tgna Esports', dot: '#f75291' },
  { match: 'TARRAGONA ESPORTS', label: 'Tgna Esports', dot: '#f75291' },
  { match: ' TE',               label: 'Tgna Esports', dot: '#f75291' }, // espais per evitar fals positiu
  { match: 'TE ',               label: 'Tgna Esports', dot: '#f75291' },
  { match: 'GU',                label: 'GU',            dot: '#e9473f' },
  { match: 'ROMAIN',            label: 'Romain',        dot: '#45ac34' },
  { match: 'MOSSOS',            label: 'Mossos',        dot: '#D0E13D' },
  { match: 'PROSEÑAL',          label: 'Proseñal',      dot: '#d4a000' },
];
```

**Separadors de múltiples responsables:** `+` i `/` (el guió `-` NO separa).

**Bug fix aplicat:** El matching usa `' '+r.toUpperCase()+' '` (amb espais al voltant) per evitar que paraules com "PROSEÑAL" activin el match de "TE".

---

## Sistema d'estats de tasques

Afegit en sessió 4/7/2026. Columna H de l'Excel.

| Valor | Comportament | Dia futur | Dia passat |
|-------|-------------|-----------|------------|
| `FET` | Fons verd suau + pill "Fet" + línia verda baix | Colorit, opac 0.72 | Gris complet |
| `EN PROCÉS` | Fons groc suau + pill "En procés" + línia groga animada | Colorit | Gris + pill groga en color (overlay) |
| `PENDENT` | Fons vermell molt suau + pill "Pendent" | Colorit | Gris + pill vermella en color (overlay) |
| *(buit)* | Sense indicador visual | Normal | Gris complet |

> El truc tècnic: les pills en color en dies passats s'aconsegueix renderitzant el badge fora del div que té `filter: grayscale()`, posicionat absolutament per sobre.

---

## Colors del Brandbook

| Color | Hex | Ús |
|-------|-----|-----|
| Blau fosc | `#0d0d4a` | Header, fons principal |
| Blau cel | `#00b2e3` | Color principal, ASO |
| Blanc | `#ffffff` | Fons, textos |
| Verd | `#45ac34` | Romain, FET |
| Groc | `#fee803` | Proseñal, EN PROCÉS |
| Vermell | `#e9473f` | GU, dia de sortida |
| Rosa | `#f75291` | Tgna Esports |
| Llima | `#D0E13D` | Mossos |
| Ambre | `#F59F27` | Default/desconegut |

> El gris és exclusivament per dies passats ("Finalitzats").

---

## Bugs resolts (no tornar a introduir)

| Bug | Causa | Solució |
|-----|-------|---------|
| Ordenació hores ("9:00" > "10:00") | Comparació de strings | `timeToMin()`: conversió a minuts |
| Fals positiu "TE" en responsables | `includes('TE')` sense límits | Matching amb espais: `' TE '` |
| Caràcters invisibles en responsables | Encoding del CSV | `.trim().toUpperCase()` agressiu |
| Dia 5 juliol no detectat | Accent en "juliol" vs "julio" | `normStr()` + RACE_DAY_CFG multilingüe |
| Animació de refresc no girava | Typo: `rotate(360eg)` | Corregit a `rotate(360deg)` |

---

## Deploy i flux de treball

```
Excel local → Google Drive → Google Sheets (CSV automàtic) → App (fetch en temps real)
                                                                        ↑
index.html local → Test localhost:8080 → Pujar a Vercel (timelinev4) ──┘
```

> Vercel `timelinev4` NO està connectat a GitHub. El deploy és manual: pujar l'`index.html` directament.
