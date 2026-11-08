# jd-framework

Framework CSS minimo e riusabile per i progetti Astro. Copre solo tre cose: **spaziature, tipografia e layout**.

- Niente componenti (bottoni, card, form, menu), niente colori decorativi, niente effetti: cambiano da progetto a progetto.
- Mobile-first, CSS puro, nessuna dipendenza, nessun preprocessore, nessuna build: la minificazione la fa Astro.
- Scale fluide in stile [Utopia](https://utopia.fyi), primitive di layout in stile [Every Layout](https://every-layout.dev).
- Tutto in `rem`: rispetta la dimensione del testo scelta dall'utente.
- `framework.css` pesa circa 3 KB minificato.

Per vedere tutte le scale e le classi, apri `demo/index.html` nel browser, senza build.

## Struttura

| File | Contenuto |
|---|---|
| `framework.css` | La **struttura**: legge solo variabili `--jd-`, nessun valore scritto a mano. Non si modifica nei progetti. |
| `tokens.default.css` | I **valori** di default, neutri. Ogni progetto lo copia in `src/styles/tokens.css` e lo personalizza. |
| `demo/index.html` | Pagina di verifica di scale e layout. |

## Installazione in un progetto Astro

```sh
pnpm add github:jovadd/jd-framework#v1.0.0
cp node_modules/jd-framework/tokens.default.css src/styles/tokens.css
```

Nel layout base (es. `src/layouts/Base.astro`), importa i fogli **in quest'ordine**:

```astro
---
import '../styles/tokens.css';          // valori del progetto
import 'jd-framework/framework.css';    // struttura (non si modifica)
import '../styles/site.css';            // componenti ed eccezioni del progetto
---
```

### Cascade layers

`framework.css` dichiara l'ordine dei layer:

```css
@layer jd.reset, jd.base, jd.layout, components, site;
```

Il framework vive solo in `jd.reset`, `jd.base` e `jd.layout`. Gli stili del progetto vanno in `components` e `site`, che vengono dopo: vincono sempre, senza `!important` e senza alzare la specificità.

```css
/* src/styles/site.css */
@layer components {
  .card { padding: var(--jd-space-m); }
}
@layer site {
  /* eccezioni puntuali del progetto */
}
```

Nei componenti `.astro` con `<style>` scoped, avvolgi le regole in `@layer components { … }`.
Le regole fuori da qualsiasi layer vincono su tutti i layer: funzionano lo stesso, ma è meglio essere espliciti.

## Token

Tutti i token sono in `tokens.default.css`. I valori fluidi vanno da 360px a 1440px, e fuori da questo intervallo restano fermi.

| Gruppo | Token |
|---|---|
| Testo | `--jd-text--2` … `--jd-text-7` |
| Spazi | `--jd-space-3xs` … `--jd-space-3xl` e coppie `--jd-space-a-b` |
| Font | `--jd-font-heading`, `--jd-font-body`, `--jd-font-mono` (solo la famiglia: il caricamento self-hosted lo fa il progetto) |
| Ritmo | `--jd-leading-*`, `--jd-tracking-*`, `--jd-weight-*`, `--jd-code-size` |
| Layout | `--jd-container-*`, `--jd-gutter`, `--jd-measure` |
| Colori | `--jd-color-text`, `--jd-color-bg` (gli unici, servono al reset) |
| Focus | `--jd-focus-color`, `--jd-focus-width`, `--jd-focus-offset`, `--jd-border-width` |
| Classi | default delle variabili delle classi di layout (`--jd-stack-gap`, `--jd-grid-min`, …) |

Uso della scala tipografica negli elementi:

| Passo | Elemento |
|---|---|
| `--jd-text-7`, `--jd-text-6` | display (titoli hero): non assegnati, si usano nei componenti |
| `--jd-text-5` … `--jd-text-1` | `h1` … `h5` |
| `--jd-text-0` | `body`, `p`, `h6` |
| `--jd-text--1` | `small` |
| `--jd-text--2` | note, didascalie (non assegnato) |

### Tabella completa (valori a 360px e a 1440px)

| Token | 360px | 1440px |
|---|---|---|
| `--jd-text--2` | 11.1px | 12.5px |
| `--jd-text--1` | 13.3px | 15px |
| `--jd-text-0` | 16px | 18px |
| `--jd-text-1` | 19.2px | 24px |
| `--jd-text-2` | 23px | 32px |
| `--jd-text-3` | 27.6px | 42.6px |
| `--jd-text-4` | 33.2px | 56.8px |
| `--jd-text-5` | 39.8px | 75.8px |
| `--jd-text-6` | 47.8px | 101px |
| `--jd-text-7` | 57.3px | 134.6px |
| `--jd-space-3xs` | 4px | 5px |
| `--jd-space-2xs` | 8px | 10px |
| `--jd-space-xs` | 12px | 15px |
| `--jd-space-s` | 16px | 20px |
| `--jd-space-m` | 24px | 30px |
| `--jd-space-l` | 32px | 40px |
| `--jd-space-xl` | 48px | 60px |
| `--jd-space-2xl` | 64px | 80px |
| `--jd-space-3xl` | 96px | 120px |
| `--jd-space-3xs-2xs` | 4px | 10px |
| `--jd-space-2xs-xs` | 8px | 15px |
| `--jd-space-xs-s` | 12px | 20px |
| `--jd-space-s-m` | 16px | 30px |
| `--jd-space-m-l` | 24px | 40px |
| `--jd-space-l-xl` | 32px | 60px |
| `--jd-space-xl-2xl` | 48px | 80px |
| `--jd-space-2xl-3xl` | 64px | 120px |
| `--jd-space-s-l` | 16px | 40px |
| `--jd-space-m-xl` | 24px | 60px |
| `--jd-space-l-2xl` | 32px | 80px |
| `--jd-space-xl-3xl` | 48px | 120px |
| `--jd-font-heading` | system-ui, sans-serif | system-ui, sans-serif |
| `--jd-font-body` | system-ui, sans-serif | system-ui, sans-serif |
| `--jd-font-mono` | ui-monospace, "SF Mono", Menlo, Consolas, monospace | ui-monospace, "SF Mono", Menlo, Consolas, monospace |
| `--jd-leading-tight` | 1.15 | 1.15 |
| `--jd-leading-normal` | 1.6 | 1.6 |
| `--jd-leading-loose` | 1.8 | 1.8 |
| `--jd-tracking-tight` | -0.02em | -0.02em |
| `--jd-tracking-normal` | 0 | 0 |
| `--jd-tracking-wide` | 0.05em | 0.05em |
| `--jd-weight-regular` | 400 | 400 |
| `--jd-weight-bold` | 700 | 700 |
| `--jd-code-size` | 0.9em | 0.9em |
| `--jd-container-max` | 75rem (1200px) | 75rem (1200px) |
| `--jd-container-narrow` | 45rem (720px) | 45rem (720px) |
| `--jd-container-wide` | 90rem (1440px) | 90rem (1440px) |
| `--jd-gutter` | 16px *(space-s-l)* | 40px *(space-s-l)* |
| `--jd-measure` | 65ch | 65ch |
| `--jd-color-text` | #1a1a1a | #1a1a1a |
| `--jd-color-bg` | #ffffff | #ffffff |
| `--jd-focus-color` | var(--jd-color-text) | var(--jd-color-text) |
| `--jd-focus-width` | 0.1875rem (3px) | 0.1875rem (3px) |
| `--jd-focus-offset` | 0.125rem (2px) | 0.125rem (2px) |
| `--jd-border-width` | 0.125rem (2px) | 0.125rem (2px) |
| `--jd-section-space` | 32px *(space-l-2xl)* | 80px *(space-l-2xl)* |
| `--jd-stack-gap` | 24px *(space-m)* | 30px *(space-m)* |
| `--jd-cluster-gap` | 16px *(space-s)* | 20px *(space-s)* |
| `--jd-cluster-justify` | flex-start | flex-start |
| `--jd-cluster-align` | center | center |
| `--jd-grid-min` | 16rem (256px) | 16rem (256px) |
| `--jd-grid-gap` | 24px *(space-m)* | 30px *(space-m)* |
| `--jd-sidebar-width` | 20rem (320px) | 20rem (320px) |
| `--jd-sidebar-content-min` | 50% | 50% |
| `--jd-sidebar-gap` | 24px *(space-m)* | 30px *(space-m)* |
| `--jd-switcher-threshold` | 40rem (640px) | 40rem (640px) |
| `--jd-switcher-gap` | 24px *(space-m)* | 30px *(space-m)* |
| `--jd-flow-space` | 16px *(space-s)* | 20px *(space-s)* |
| `--jd-flow-heading-space` | 32px *(space-l)* | 40px *(space-l)* |

## Classi di layout

Le classi seguono il formato BEM con prefisso `jd-`. Ognuna si regola con una **variabile locale** invece che con tante varianti:

```html
<div class="jd-stack" style="--jd-stack-gap: var(--jd-space-l)">…</div>
```

I default delle variabili stanno in `tokens.css`: cambiali lì per tutto il progetto, oppure su un singolo elemento con `style` o con una classe del layer `components`.
Le variabili si ereditano: un elemento annidato con la stessa classe riceve il valore del genitore, a meno che non ne imposti uno suo.

Tutte le classi usano solo proprietà logiche (`margin-inline`, `padding-block`, …).

### `.jd-container`

Centra il contenuto con una larghezza massima e aggiunge il gutter laterale. La larghezza massima è quella del **contenuto**, gutter escluso (`box-sizing: content-box`).

| Variabile | Default |
|---|---|
| `--jd-container-max` | `75rem` |
| `--jd-gutter` | `var(--jd-space-s-l)` |

Modificatori: `.jd-container--narrow` (`--jd-container-narrow`, 45rem) e `.jd-container--wide` (`--jd-container-wide`, 90rem).

```astro
<section class="jd-section">
  <div class="jd-container jd-container--narrow">
    <slot />
  </div>
</section>
```

### `.jd-section`

Aggiunge il padding verticale di una sezione di pagina.

| Variabile | Default |
|---|---|
| `--jd-section-space` | `var(--jd-space-l-2xl)` (32 → 80px) |

Modificatori: `.jd-section--s` (`--jd-space-m-xl`, 24 → 60px) e `.jd-section--l` (`--jd-space-xl-3xl`, 48 → 120px).

```astro
<section class="jd-section jd-section--l">
  <div class="jd-container"><h2>{title}</h2></div>
</section>
```

### `.jd-stack`

Dispone i figli in colonna, con uno spazio uniforme tra l'uno e l'altro (`gap`).

| Variabile | Default |
|---|---|
| `--jd-stack-gap` | `var(--jd-space-m)` |

```astro
<article class="card jd-stack" style="--jd-stack-gap: var(--jd-space-xs)">
  <h3>{title}</h3>
  <p>{excerpt}</p>
</article>
```

### `.jd-cluster`

Dispone i figli in riga e li manda a capo quando lo spazio finisce. Va bene per menu, tag e gruppi di bottoni.

| Variabile | Default |
|---|---|
| `--jd-cluster-gap` | `var(--jd-space-s)` |
| `--jd-cluster-justify` | `flex-start` |
| `--jd-cluster-align` | `center` |

```astro
<nav class="jd-cluster" style="--jd-cluster-justify: space-between">
  <a href="/">Logo</a>
  <ul class="jd-cluster" role="list">{links.map(l => <li><a href={l.href}>{l.label}</a></li>)}</ul>
</nav>
```

### `.jd-grid`

Griglia `auto-fit` in cui ogni colonna non scende mai sotto `--jd-grid-min`. Le colonne si riducono da sole, senza media query. Su schermi più stretti del minimo la colonna occupa il 100%.

| Variabile | Default |
|---|---|
| `--jd-grid-min` | `16rem` |
| `--jd-grid-gap` | `var(--jd-space-m)` |

```astro
<ul class="jd-grid" role="list" style="--jd-grid-min: 20rem">
  {posts.map(p => <li><Card {...p} /></li>)}
</ul>
```

### `.jd-sidebar`

Due colonne: il **primo figlio** è la sidebar a larghezza fissa, il secondo occupa lo spazio rimanente. Quando il contenuto scenderebbe sotto `--jd-sidebar-content-min`, le due colonne si impilano.
Con `.jd-sidebar--end` la sidebar diventa l'**ultimo** figlio.

| Variabile | Default |
|---|---|
| `--jd-sidebar-width` | `20rem` |
| `--jd-sidebar-content-min` | `50%` |
| `--jd-sidebar-gap` | `var(--jd-space-m)` |

```astro
<div class="jd-sidebar jd-sidebar--end" style="--jd-sidebar-width: 18rem">
  <main class="jd-flow"><slot /></main>
  <aside>…</aside>
</div>
```

### `.jd-switcher`

Mostra i figli in colonna quando il contenitore è più stretto di `--jd-switcher-threshold` e in riga quando è più largo. Tutti i figli cambiano insieme: non ci sono stati intermedi, per esempio 2 + 1.

| Variabile | Default |
|---|---|
| `--jd-switcher-threshold` | `40rem` |
| `--jd-switcher-gap` | `var(--jd-space-m)` |

```astro
<div class="jd-switcher" style="--jd-switcher-threshold: 50rem">
  <Feature /><Feature /><Feature />
</div>
```

### `.jd-flow`

Aggiunge spazio tra gli elementi di un testo lungo (privacy, chi siamo, articoli) e allarga lo spazio prima di `h2` e `h3`. Limita anche la riga a `--jd-measure` per la leggibilità.

| Variabile | Default |
|---|---|
| `--jd-flow-space` | `var(--jd-space-s)` |
| `--jd-flow-heading-space` | `var(--jd-space-l)` |
| `--jd-measure` | `65ch` |

```astro
<article class="jd-flow">
  <Content />
</article>
```

### `.jd-full-bleed`

Fa uscire l'elemento dal container, da bordo a bordo del viewport, a qualsiasi profondità. Lo scroll orizzontale causato dalla barra di scorrimento è evitato da `overflow-x: clip` su `html`, nel reset.

```astro
<div class="jd-container jd-flow">
  <p>…</p>
  <img class="jd-full-bleed" src={hero.src} alt="" />
</div>
```

### `.jd-cq`

Imposta `container-type: inline-size`: l'elemento diventa il riferimento per le container queries dei componenti che contiene.

```astro
<div class="jd-cq">
  <article class="card">…</article>
</div>

<style>
  @layer components {
    .card { display: grid; gap: var(--jd-space-s); }
    @container (min-width: 30rem) {
      .card { grid-template-columns: 8rem 1fr; }
    }
  }
</style>
```

> **Perché `.jd-sidebar`, `.jd-switcher` e `.jd-grid` non usano `@container`.** Le condizioni di `@container` non accettano `var()`, quindi la soglia non si potrebbe regolare con una variabile. Queste tre classi usano le tecniche intrinseche di Every Layout (flex-basis/grow e `auto-fit`). Queste tecniche reagiscono già alla larghezza del genitore, non del viewport, quindi si comportano come container queries con soglia regolabile.

## Personalizzare un progetto

1. Copia `node_modules/jd-framework/tokens.default.css` in `src/styles/tokens.css`.
2. Cambia i valori: font, colori, container e default delle classi.
3. **Non rinominare né togliere variabili**: il framework le legge tutte.
4. Le eccezioni vanno in `src/styles/site.css` (layer `site`) e i componenti nel layer `components`. Il framework non si tocca.

### Rigenerare le scale fluide

Le scale seguono il metodo di [utopia.fyi](https://utopia.fyi). Per ogni passo:

```
pendenza    = (maxPx − minPx) / (maxViewport − minViewport)
intercetta  = minPx − pendenza × minViewport
valore      = clamp(minPx/16 rem, intercetta/16 rem + pendenza×100 vw, maxPx/16 rem)
```

**Tipografia.** Usa il [type calculator di Utopia](https://utopia.fyi/type/calculator) con questi parametri:

| Parametro | Valore |
|---|---|
| Viewport minimo | 360px |
| Viewport massimo | 1440px |
| Base minima | 16px |
| Base massima | 18px |
| Rapporto minimo | 1.2 (terza minore) |
| Rapporto massimo | 1.333 (quarta giusta) |
| Passi positivi | 7 |
| Passi negativi | 2 |

Fai un'eccezione per i passi negativi `-1` e `-2`: calcolali con il rapporto 1.2 anche a 1440px, cioè `18 / 1.2ⁿ`. Con 1.333 sarebbero più piccoli su desktop che su mobile.

**Spaziature.** Usa il [space calculator di Utopia](https://utopia.fyi/space/calculator) con questi parametri:

| Parametro | Valore |
|---|---|
| Viewport | 360 → 1440px |
| Base | 16 → 20px |
| 3xs | 0.25 |
| 2xs | 0.5 |
| xs | 0.75 |
| s | 1 |
| m | 1.5 |
| l | 2 |
| xl | 3 |
| 2xl | 4 |
| 3xl | 6 |

Una coppia `--jd-space-a-b` va dal valore minimo di `a` al valore massimo di `b`.

I parametri sono scritti anche nei commenti di `tokens.default.css`.

## Aggiornare un progetto a una nuova versione

1. Leggi `CHANGELOG.md`. Con una **major** (es. 2.0.0) controlla token e classi rinominati o rimossi.
2. Cambia il tag della dipendenza:
   ```sh
   pnpm add github:jovadd/jd-framework#v1.1.0
   ```
3. Se la versione aggiunge token, copia le nuove variabili da `node_modules/jd-framework/tokens.default.css` nel tuo `tokens.css`.
4. Controlla il sito in locale e fai il deploy.

### Pubblicare una nuova versione del framework

1. Modifica `framework.css` e/o `tokens.default.css` e verifica con `demo/index.html`.
2. Aggiorna `version` in `package.json` e la riga della versione in testa a `framework.css`.
3. Aggiungi una voce in `CHANGELOG.md`.
4. Fai commit, tag e push:
   ```sh
   git commit -am "…" && git tag vX.Y.Z && git push && git push --tags
   ```

## Regole d'uso (da copiare nel CLAUDE.md dei progetti)

```markdown
## CSS: jd-framework

- Il progetto usa jd-framework (github:jovadd/jd-framework), importato nel layout base in quest'ordine:
  src/styles/tokens.css → jd-framework/framework.css → src/styles/site.css
- NON modificare il framework nel progetto: niente modifiche in node_modules/jd-framework, niente copie di framework.css.
  Se serve una modifica strutturale, va fatta nel repository jd-framework e pubblicata con una nuova versione.
- I valori (font, colori, container, spaziature, default delle classi) si cambiano solo in src/styles/tokens.css,
  senza rinominare né togliere variabili --jd-.
- I componenti vanno nel layer `components`: `@layer components { … }` (anche dentro <style> dei file .astro).
- Le eccezioni puntuali vanno in src/styles/site.css, nel layer `site`.
- Mai `!important` per battere il framework: i layer components e site vincono già.
- Usa le classi di layout del framework (jd-container, jd-section, jd-stack, jd-cluster, jd-grid, jd-sidebar,
  jd-switcher, jd-flow, jd-full-bleed, jd-cq) e regolale con le loro variabili, es. style="--jd-stack-gap: var(--jd-space-l)".
- Spaziature e dimensioni del testo sempre dai token (var(--jd-space-*), var(--jd-text-*)), mai valori scritti a mano.
- Non creare classi utility di testo o di margine (es. mt-4, text-lg).
```

## Licenza

MIT
