# Pendents Timeline Tour de França 2026

**Última actualització:** 4 juliol 2026, ~02:30h

---

## ✅ COMPLETAT (sessió 4/7/2026)

- [x] **Bug CSS animació refresc** — `rotate(360eg)` → `rotate(360deg)`
- [x] **Sistema d'estats de tasques** — Columna H a l'Excel: `FET`, `EN PROCÉS`, `PENDENT`
  - Estils visuals per cada estat (fons suau + pill + línia inferior)
  - Dies passats: targetes grises però pills `EN PROCÉS`/`PENDENT` en color (overlay)
  - Testada localment ✅

---

## 🔴 URGENT - Pendent de fer ara

- [ ] **Pujar `index.html` a Vercel** (`timelinev4`) i verificar a `timeline.tarragonaeltouracasa.cat`
- [ ] **Responsive mini-calendari** (tasca 1) — valorar si cal fer-ho abans del 5 de juliol

---

## 🟡 PRIMERA FASE - Abans o durant el Tour (5-6 juliol)

### 1. Responsive millorat del mini-calendari
**Risc:** BAIX — Només CSS/estils

El mini-calendari de pills al nav es veu apinyat en mòbil.
- Reduir padding/marge entre pills
- Scroll horitzontal més suau
- Millorar el tap target per touch

---

## 🔵 SEGONA FASE - Després del Tour (a partir del 7 de juliol)

### PWA (Progressive Web App) — Primera tasca de segona fase
- Service Worker per cache i ús offline
- `manifest.json` per instal·lació al mòbil
- Icones per diferents dispositius

### 18. Sistema d'avisos/pop-ups configurables des de l'Excel
**Risc:** MITJÀ — Requereix disseny i testing exhaustiu

Poder posar a l'Excel un avís tipus banner/pop-up que aparegui durant una hora/dia específic.

**Proposta preferida (Opció A — columna extra):**
- Columna I: `AVIS` (text del missatge)
- Columna J: `AVIS_TIPUS` (`info`, `warning`, `error`)
- Si `AVIS` té contingut, mostrar banner a dalt de la vista del dia corresponent
- Banner amb X per tancar (persistit a `localStorage`)

**Requeriments de testing:**
- Avisos sense horari (tot el dia)
- Avisos amb data/hora passada (no mostrar)
- Múltiples avisos actius alhora
- Tancar avís no el torna a mostrar (localStorage)

### 2. Feedback visual en actualització
Toast/notificació subtil "Dades actualitzades" quan es refresca.

### 4. Mode fosc
Theme toggle clar/fosc per ús nocturn. Guardar preferència a `localStorage`.

### 5. Exportar/compartir dia
URL directa a un dia específic (`#dia-5-juliol`). Copiar al portapapers.

### 8. Lazy loading del logo
Baixa prioritat — només si s'afegeixen més imatges.

### 9. Notificacions push
Notification API per avisar quan s'apropin tasques importants.

### 10. Filtre per data range
Filtres ràpids: "Aquesta setmana", "Juny", "Juliol". Revisar compatibilitat amb navegació actual.

### 11. Vista setmanal
Alternativa a la vista per dia. Graella de múltiples dies.

### 13. Cache intel·ligent
Guardar dades a `localStorage`, mostrar-les mentre carrega del Sheets.

### 14a. Error handling millorat
Validació de columnes, fallbacks, missatges d'error descriptius.

### 14b. Loading skeleton
Rectangles animats en lloc del spinner mentre carreguen les dades.

### 15. Accessibilitat
ARIA labels, navegació per teclat, contrast WCAG AA.

---

## ❌ Descartat / No prioritari

| Tasca | Motiu |
|-------|-------|
| Persistència de filtres (localStorage) | No útil per aquest cas d'ús |
| Virtual scrolling | No necessari (poques dades) |
| Memoització agressiva | React ja optimitza bé |
| Lazy loading imatges | Només hi ha 1 imatge |

---

## 📋 Notes tècniques importants

### Flux de treball
1. Branca nova per cada feature: `git checkout -b feature/nom`
2. Testejar localment: `python3 -m http.server 8080` → `http://localhost:8080`
3. Validar amb dades reals del Google Sheets
4. Pujar `index.html` a Vercel (`timelinev4`) manualment
5. Verificar a `timeline.tarragonaeltouracasa.cat`

### Columnes actuals de l'Excel
`A: Data | B: Dia | C: Hora fi | D: Concepte | E: Obs | F: Ubicació | G: Responsable | H: Estat`

### Estat de sincronització Git/Vercel
- Vercel `timelinev4` **NO** connectat a GitHub → deploy sempre manual
- Branca `main` del repo desfasada respecte a producció
- Branca `estats-tasques` → versió actual (la correcta + nova feature)
