# Money Trees — Landing page « soirée en live »

Contexte et **direction artistique (DA)** du site. Ce fichier sert de référence à Claude Code pour continuer les intégrations. Le rendu de référence est dans **`index.html`** (page autonome, HTML/CSS/JS vanilla, sans build).

---

## 1. Contexte projet

- **Marque** : Money Trees (agence de closing / formation).
- **Objectif de la page** : funnel d'inscription à un **événement live gratuit**.
- **Événement** : **1 seule soirée**, **dimanche 6 septembre à 20h00 (heure française)**. Le caractère « une seule soirée » est un argument clé, à garder visible partout.
- **Promesse (H1)** : « La nouvelle activité qui a permis à +650 Français de sortir du métro-boulot-dodo avec confiance et sérénité, à l'ère de l'IA. » — sans se filmer, sans compétence technique.
- **Langue** : français, tutoiement.
- ⚠️ **Ne jamais réintroduire le mot « Momentum »** (ancien nom, supprimé partout).

## 2. Stack & contraintes

- HTML/CSS/JS **vanilla**, un seul fichier autonome. Pas de framework, pas de build.
- **Polices via Fontshare** (CDN) : `Clash Display` (titres) + `Satoshi` (texte/UI). Conserver le `<link>` `https://api.fontshare.com/v2/css?...`. Si le site est hébergé en prod, envisager de self-host les woff2 pour la perf.
- Tout le CSS est inline dans le `<head>` (un seul `<style>`), tous les tokens sont des **variables CSS** sous `:root`.

---

## 3. Direction artistique

### Principe
DA **premium, vert, « growth »**. Le fil rouge visuel est une **courbe ascendante** (décollage) + des **halos verts** lumineux. Le rythme alterne **sections sombres et claires** pour éviter la monotonie :

`hero (sombre) → apprendre (clair) → déroulé (sombre) → est-ce fait pour toi (gris-vert) → témoignages (clair) → footer (sombre)`

### Couleurs (tokens `:root`)

| Token | Hex | Usage |
|---|---|---|
| `--bg-dark` | `#06160F` | fond des sections sombres |
| `--ink-l` | `#F3FBF6` | texte sur fond sombre |
| `--muted-l` | `#9DB8AC` | texte secondaire sur fond sombre |
| `--green` | `#2FE07E` | vert principal (accent, CTA, nœuds) |
| `--lime` | `#A8F06A` | accent clair (dégradés) |
| `--teal` | `#23DCC2` | accent froid (dégradés, touche stylée) |
| `--cta-1` / `--cta-2` | `#38E884` / `#12A85C` | dégradé des boutons |
| `--bg-light` | `#F6FCF8` | fond des sections claires (apprendre, témoignages) |
| `--ink` | `#10201A` | texte sur fond clair |
| `--muted` | `#566B60` | texte secondaire sur fond clair |
| `--line` | `#E2F0E7` | bordures claires |
| `--mint` | `#E7F7EE` | pastilles / kickers clairs |
| `--green-dk` | `#12934F` | vert foncé (texte accent sur clair) |
| `--gold` | `#E8A93C` | étoiles (si besoin) |

Couleurs locales (pas dans `:root`) :
- Section « Est-ce fait pour toi » : fond dégradé **`#ECF3EE → #E4EFE8`** (gris-vert, volontairement distinct du `--bg-light` des témoignages).
- Sceaux : **check vert `#22B457`**, **croix rouge `#E5484D`**.
- Barré « métro-boulot-dodo » : dégradé chaud **`#f0a878 → #e8748a`**.

### Typographie
- **Titres** : `Clash Display` 500/600/700, `letter-spacing:-.02em`, `line-height:1.03`.
- **Texte/UI** : `Satoshi` 400/500/700.
- **Hero responsive (clé)** : le H1 et les espacements sont pilotés par la **hauteur d'écran** pour que la 1ʳᵉ section tienne en entier (desktop + mobile) :
  - H1 : `font-size:clamp(27px, min(7.2vw,7.1vh), 56px)`
  - gaps du hero : `gap:clamp(14px,2.9vh,28px)`
  - Ne pas remplacer ces `min(vw,vh)` par du `vw` seul, sinon le hero déborde sur écrans courts.

### Motifs signatures (à réutiliser pour rester cohérent)
- **Courbe ascendante** : SVG `.hero-line` (dégradé `#lineg`, filtre `#glow`), animée au chargement via `stroke-dashoffset`.
- **Halos** : `radial-gradient` verts/teal en fond de section (`.hero-bg`, `.night-bg`).
- **Grain** : overlay `.grain` (feTurbulence, `opacity:.05`, `mix-blend:overlay`).
- **Texte dégradé** : classe `.grad` (lime→green→teal, `background-clip:text`).
- **Soulignement manuscrit** : SVG `.uline` sous un mot clé (dégradé `#ug`).
- **Barré** : `.strike` (montre ce qu'on quitte).
- **Sceaux** : rosette « badge vérifié » (`.seal.good` vert / `.seal.bad` rouge).

### Formes & profondeur
- Rayons : boutons `13–20px`, cartes `17–26px`, tuiles/nœuds `14–17px`.
- Ombres douces teintées vert (ex. `0 30px 60px -34px rgba(16,80,50,.4)`).
- Largeur max contenu : `--maxw:1180px`, padding latéral `26px`.

---

## 4. Structure des sections (dans `index.html`)

1. **Header** — `.announce` (bannière verte, **défile** avec la page) + `.nav` (logo + CTA, **sticky** `position:sticky;top:0`). ⚠️ Ne pas remettre la bannière en sticky.
2. **Hero** `.hero` (sombre) — H1 stylé, lede, 3 `.checks-row`, `.cta-btn` avec date. Fit viewport via `min-height:calc(100svh - var(--head))`.
3. **Apprendre** `.learn` (clair) — emplacement photo + 4 `.li` (check) + CTA.
4. **Déroulé de la soirée** `.night` (sombre) — copy + `.timeline` verticale (4 `.tl-item`, nœuds `.tl-node` glow, chips horaires).
5. **Est-ce fait pour toi** `.fit` (gris-vert) — 2 colonnes `.fit-card.good` / `.fit-card.bad`, sceaux verts/rouges.
6. **Témoignages** `.tmo` (clair) — carrousel `.tcarousel` (flèches, dots, captures d'écran réelles en boucle infinie).
7. **CTA final** `.final` (sombre) — gros call-to-action juste avant le footer : kicker, H2, `.checks-row`, `.cta-btn`, note de réassurance. Réutilise les classes globales `.checks-row`/`.cta-btn`/`.grad` du hero, ne pas les re-scoper.
8. **Footer** `footer` (sombre, **contenu centré**) — logo, `© 2026 Money Trees Agency · Mentions légales · Politique de confidentialité`, mentions Meta + disclaimer résultats.

## 5. Composants & classes utiles
- **Boutons** : `.btn` (vert, standard), `.cta-btn` (gros bouton hero avec sous-titre `.cta-sub`).
- **Kicker/pastille** : `.kick` (clair) / `.eyebrow` (variantes).
- **Cartes** : `.li` (apprendre), `.fit-card` (qualif), `.tcard` (témoignage).
- **Timeline** : `.timeline > .tl-item > (.tl-rail>.tl-node) + (.tl-c>.tl-time,.tl-t,.tl-d)`.
- **Carrousel** : `#ttrack` (piste), `.tcard.active` (carte centrée), `#tprev`/`#tnext`, `#tdots`.

## 6. Comportements JS (déjà en place, tout en bas du fichier)
- `--head` : recalcule dynamiquement la hauteur de `.announce` + `.nav` (au chargement + au `resize`) et la stocke en variable CSS, pour que le hero (`min-height:calc(100svh - var(--head))`) tienne toujours sous le header sticky.
- **Courbe animée** (`.draw`) : au chargement, dessin progressif de la courbe ascendante via `stroke-dasharray`/`stroke-dashoffset` (transition ~1.9s). Désactivé si `prefers-reduced-motion: reduce`.
- **Reveal au scroll** : `IntersectionObserver` ajoute `.in` sur tout élément `.reveal` dès qu'il entre dans le viewport (`threshold:.12`), puis se désabonne (animation one-shot).
- **Carrousel témoignages** : piste `#ttrack` translatée en JS pour centrer la carte active (`idx`), dots générés dynamiquement, boutons `#tprev`/`#tnext` pour naviguer (boucle infinie), re-centrage au `resize` et au `load`.

---

## 7. Assets disponibles dans le dossier

Le dossier contient déjà des images à intégrer lors des prochaines itérations (ne pas les dupliquer, ne pas les renommer sans prévenir) :
- `Fond transparent Copie de Logo Money Trees.png` — logo officiel (à utiliser à la place du logo SVG générique dans `.brand`/`.foot-brand` si demandé).
- `Photo William 2.png`, `William photo .png` — photos à utiliser potentiellement en section `.learn` (`.photo`) à la place du placeholder actuel.
- `tesi elite 5.png`, `testi elite 1 .png`, `testi elite 2.png`, `testi elite 3.png`, `testi elite 4.png`, `testi elite 6.png`, `testi elite 7.png`, `testi elite 8.png` — captures de témoignages, probablement destinées à remplacer ou compléter le carrousel `.tmo` / `#ttrack`.

## 8. Règles de travail pour les prochaines modifs

- Toujours modifier `index.html` en place (fichier unique, pas de nouveaux fichiers CSS/JS séparés sauf demande explicite).
- Respecter les tokens `:root` existants plutôt que d'introduire de nouvelles couleurs en dur.
- Garder l'alternance sombre/clair des sections telle quelle sauf demande contraire.
- Ne pas casser le fit viewport du hero (`--head`, `clamp(...vh...)`) en modifiant le texte du H1/lede.
- Ne jamais réintroduire « Momentum ».
