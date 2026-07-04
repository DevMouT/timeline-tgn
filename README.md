# Timeline Tour de França 2026 - Tarragona

Aplicació web de timeline i calendari per gestionar la **2a Etapa del Tour de França 2026**, amb sortida de Tarragona el 5 de juliol de 2026.

## Què fa

Mostra totes les tasques i accions organitzatives per dies i responsables, carregant les dades en temps real d'un Google Sheets públic. L'usa l'equip organitzador de Tarragona Esports (>20 persones).

## Accés

- **Producció:** [timeline.tarragonaeltouracasa.cat](https://timeline.tarragonaeltouracasa.cat)
- **Repositori:** [github.com/DevMouT/timeline-tour2](https://github.com/DevMouT/timeline-tour2)
- **Vercel (projecte actiu):** `timelinev4` — deploy manual (no connectat a GitHub)

## Dades

Les dades vénen d'un Google Sheets públic. El flux és:

1. Editar `TIMELINE TOUR.xlsx` en local
2. Reemplaçar el fitxer a Google Drive
3. L'app llegeix el CSV exportat automàticament en temps real

**Sheet ID:** `15O6ZoZjfTnzZKRDgGUZBNzuD88KyfvvJ`

## Tecnologia

HTML únic (`index.html`) sense build step:
- React 18 via ESM (esm.sh)
- Tailwind CSS 4 via CDN
- Babel Standalone per JSX al navegador
- lucide-react per icones

## Desenvolupament local

```bash
# Servidor local (necessari per CORS amb fetch)
python3 -m http.server 8080
# Obre http://localhost:8080
```

> Nota: obrir `index.html` directament amb doble clic falla perquè el navegador bloqueja el `fetch()` des de `file://`. Cal el servidor local.

## Deploy

Pujar l'`index.html` manualment al projecte `timelinev4` de Vercel.
