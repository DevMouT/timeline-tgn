# Handoff - Timeline Tour de França 2026

> Document de context per a noves sessions de treball. Actualitzar al final de cada sessió.

---

## Estat del projecte

**Data última actualització:** 4 juliol 2026, ~02:30h
**Branca activa:** `estats-tasques`
**Estat general:** ✅ Funcional en producció. Nova feature d'estats implementada i testada localment, pendent de pujar a Vercel.

---

## On estem ara mateix

### Fet en aquesta sessió (4/7/2026)

1. **Investigació inicial** — Descobert que la versió local del repo NO era la correcta. La versió real estava pujada manualment a Vercel `timelinev4`. L'hem descarregat i substituït.

2. **Bug CSS corregit** — `rotate(360eg)` → `rotate(360deg)`. L'animació del botó de refresc no girava.

3. **Sistema d'estats implementat** (branca `estats-tasques`):
   - Columna H de l'Excel: `FET`, `EN PROCÉS`, `PENDENT`, o buit
   - Estils visuals per a cada estat (fons de color suau + pill + línia inferior)
   - A dies passats: targetes en gris, però pills de `EN PROCÉS` i `PENDENT` en color (via overlay absolut, l'únic truc que permet trencar el CSS grayscale del pare)
   - Testada localment a `http://localhost:8080` ✅

4. **Documentació creada** — README.md, Arquitectura.md, Handoff.md, PENDENTS.md actualitzat

### Pendent de fer ara mateix

- [ ] Pujar `index.html` a Vercel (`timelinev4`) manualment
- [ ] Verificar a `timeline.tarragonaeltouracasa.cat`
- [ ] Decidir si fem el responsive del mini-calendari (tasca 1) abans del 5 de juliol

---

## Contexte crític que cal saber

### ⚠️ Vercel NO connectat a GitHub
El projecte `timelinev4` de Vercel (el que serveix `timeline.tarragonaeltouracasa.cat`) **no té el repo GitHub connectat**. El deploy és sempre manual: pujar l'`index.html` directament a Vercel.

Si algun dia es vol connectar: Vercel → `timelinev4` → Settings → Git → connectar `DevMouT/timeline-tour2`.

### ⚠️ No obrir index.html amb doble clic
El `fetch()` des de `file://` falla per CORS. Per testejar en local cal:
```bash
cd /Users/oskarpujol/timeline-tour2 && python3 -m http.server 8080
```
I obrir `http://localhost:8080`.

### ⚠️ L'app està en ús per >20 persones
Màxima prudència. Cap canvi a producció sense testejar localment primer.

### ⚠️ Excel → Drive → Sheets → CSV
El flux de dades és: editar `TIMELINE TOUR.xlsx` en local → reemplaçar a Google Drive → l'app llegeix el CSV exportat automàticament. No es treballa directament amb Google Sheets.

---

## Estructura de l'Excel (columnes)

| Col | Índex | Contingut |
|-----|-------|-----------|
| A | r[0] | Data `DD/MM/YYYY` (capçalera de dia) |
| B | r[1] | Nom del dia |
| C | r[2] | HORA FI |
| D | r[3] | CONCEPTE |
| E | r[4] | OBSERVACIONS |
| F | r[5] | UBICACIÓ |
| G | r[6] | RESPONSABLE |
| H | r[7] | ESTAT ← nova, afegida 4/7/2026 |

---

## Branques Git

| Branca | Estat | Contingut |
|--------|-------|-----------|
| `main` | Producció (però desfasada) | Versió antiga, NO és la live |
| `estats-tasques` | En curs | Sistema d'estats + bug CSS fix |

> Nota: `main` al GitHub NO és la versió live. La versió live es va pujar manualment a Vercel en un moment anterior. Caldrà sincronitzar quan es pugui.

---

## Responsables i colors

| Responsable | Match al CSV | Color |
|-------------|-------------|-------|
| ASO | `ASO` | `#00b2e3` blau cel |
| Tgna Esports | `TGNA ESPORTS`, `TARRAGONA ESPORTS`, ` TE`, `TE ` | `#f75291` rosa |
| GU | `GU` | `#e9473f` vermell |
| Romain | `ROMAIN` | `#45ac34` verd |
| Mossos | `MOSSOS` | `#D0E13D` llima |
| Proseñal | `PROSEÑAL` | `#d4a000` groc fosc |
| Default | qualsevol altre | `#F59F27` ambre |

**Important:** El matching usa `' '+r.toUpperCase()+' '` (espais al voltant) per evitar fals positiu amb "TE" dins de paraules.

---

## Normes de treball acordades

- ⛔ Cap canvi sense confirmar primer amb l'usuari
- 🌿 Sempre branca nova, mai directament a `main`
- 🧪 Testejar localment (localhost:8080) abans de pujar a Vercel
- 📝 Documentar canvis fets al final de cada sessió en aquest Handoff

---

## Arxius del projecte

```
timeline-tour2/
├── index.html              ← App completa (l'únic fitxer de codi)
├── TIMELINE TOUR.xlsx      ← Excel de dades (no es publica a GitHub)
├── README.md               ← Descripció del projecte
├── Arquitectura.md         ← Documentació tècnica
├── Handoff.md              ← Aquest fitxer (context entre sessions)
├── PENDENTS.md             ← Tasques pendents i roadmap
└── Prompt Agent Cursor - Timeline Tour de França 2026.md  ← Brief original
```

---

## Historial de sessions

### Sessió 1 — 4 juliol 2026 (~00:00h - ~02:30h)
- Lectura del brief i investigació de l'estat del projecte
- Descoberta discrepància local vs. Vercel (2KB de diferència)
- Substitució de l'`index.html` local per la versió correcta de producció
- Implementació del sistema d'estats (FET/EN PROCÉS/PENDENT)
- Correcció bug CSS animació refresc
- Creació documentació completa
- **Pendent:** Pujar a Vercel
