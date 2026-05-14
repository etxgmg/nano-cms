# Bygg en webbplats för nano-cms
### En guide för dig som vajbkodar — med Claude, Cursor eller annat AI-stöd

---

## Vad är din roll?

Du bygger den **publika webbplatsen** — layout, design, typografi, känsla. Du bestämmer allt visuellt.

**nano-cms** hanterar det redigerbara innehållet: text och bilder som sajtägaren ska kunna ändra från sin mobil utan att förstå kod.

Det enda kravet på dig är ett **kontrakt**: all synlig text och alla synliga bilder måste hämtas från `content.json`. Ingenting hårdkodas i HTML.

---

## Filerna du arbetar med

```
repo/
├── index.html          ← din sajt (du bygger den)
├── content.json        ← innehållet (du definierar strukturen)
├── admin/
│   └── index.html      ← adminens gränssnitt (rör inte)
└── bilder/             ← bilder hamnar här automatiskt
```

Du äger `index.html` och `content.json`. Rör ingenting i `admin/`.

---

## content.json — innehållskontraktet

`content.json` är gränssnittet mellan dig och sajtägaren. Du **definierar** strukturen en gång. Sajtägaren **redigerar** värden senare.

### Grundformat

```json
{
  "blocks": [
    {
      "id":    "hero",
      "type":  "imageText",
      "label": "Översta delen",
      "editable": {
        "title":      "Välkommen",
        "text":       "Vi hjälper dig med allt.",
        "image":      "bilder/hero.jpg",
        "alt":        "Framsidesbild",
        "buttonText": "Ring oss",
        "buttonHref": "tel:+46701234567"
      }
    }
  ]
}
```

### Tre regler för fält i `editable`

1. **Namnge efter vad det är**, inte var det syns. `"title"` inte `"hero-heading"`.
2. **Bildfält heter `image`, `img` eller slutar på dessa.** Adminen känner igen dem och visar bildväljare.
3. **Lång text heter `text`, `body`, `description` eller `quote`.** Adminen visar då ett flerlinjigt fält.

### Fältnamn och vad adminen gör med dem

| Ditt fältnamn                             | Adminen visar          |
|-------------------------------------------|------------------------|
| `title`, `heading`, `subtitle`            | Textfält (enrad)       |
| `text`, `body`, `description`, `quote`    | Textfält (flerrad)     |
| `image`, `img`, `photo`, `*image`, `*img` | Bildväljare + förhandsvisning |
| `alt`                                     | Textfält (bildbeskrivning) |
| `buttonText`, `linkText`                  | Textfält (enrad)       |
| `buttonHref`, `href`, `url`, `*url`       | URL-fält               |
| `email`                                   | E-postfält             |
| `tel`, `phone`                            | Telefonfält            |
| Allt annat                                | Textfält (enrad)       |

---

## Hämta innehållet i index.html

Ladda `content.json` med `fetch` vid sidladdning. Allt ska renderas dynamiskt.

```html
<div id="site"></div>

<script>
  fetch('./content.json')
    .then(r => r.json())
    .then(data => renderSite(data));

  function renderSite(data) {
    data.blocks.forEach(block => {
      const e = block.editable;
      // Rendera varje block baserat på block.id eller block.type
    });
  }
</script>
```

### Rendera ett specifikt block via id

```js
function renderSite(data) {
  const hero    = data.blocks.find(b => b.id === 'hero');
  const about   = data.blocks.find(b => b.id === 'about');
  const contact = data.blocks.find(b => b.id === 'contact');

  document.getElementById('hero-title').textContent   = hero.editable.title;
  document.getElementById('hero-text').textContent    = hero.editable.text;
  document.getElementById('hero-img').src             = hero.editable.image;
  document.getElementById('hero-img').alt             = hero.editable.alt;
  document.getElementById('hero-btn').textContent     = hero.editable.buttonText;
  document.getElementById('hero-btn').href            = hero.editable.buttonHref;
  // osv.
}
```

### Rendera alla block dynamiskt (blocktyp-baserat)

```js
function renderSite(data) {
  const site = document.getElementById('site');

  data.blocks.forEach(block => {
    const e   = block.editable;
    const sec = document.createElement('section');
    sec.id    = block.id;

    if (block.type === 'imageText') {
      sec.innerHTML = `
        ${e.image ? `<img src="${e.image}" alt="${e.alt ?? ''}">` : ''}
        <div>
          <h2>${e.title ?? ''}</h2>
          <p>${e.text ?? ''}</p>
          ${e.buttonText ? `<a href="${e.buttonHref ?? '#'}">${e.buttonText}</a>` : ''}
        </div>
      `;
    }

    if (block.type === 'paragraph') {
      sec.innerHTML = `
        <h2>${e.title ?? ''}</h2>
        <p>${e.text ?? ''}</p>
      `;
    }

    if (block.type === 'contact') {
      sec.innerHTML = `
        <h2>${e.title ?? ''}</h2>
        <p>${e.text ?? ''}</p>
        ${e.tel   ? `<a href="tel:${e.tel}">${e.tel}</a>` : ''}
        ${e.email ? `<a href="mailto:${e.email}">${e.email}</a>` : ''}
      `;
    }

    site.appendChild(sec);
  });
}
```

---

## Promptmall om du vajbkodar med AI

Kopiera och klistra in detta till Claude, Cursor eller liknande när du ber om hjälp med sajten:

```
Jag bygger en webbplats som hämtar allt innehåll från content.json via fetch().
Strukturen på content.json ser ut så här:

[klistra in din content.json]

Regler jag måste följa:
- Ingen synlig text får hårdkodas i HTML — allt ska läsas från content.json
- Ingen synlig bild får hårdkodas — bildvägar ska läsas från content.json
- Fältnamnen i "editable" får inte ändras — adminen beror på dem
- block.id:n är stabila identifierare — ändra dem inte

Hjälp mig med: [beskriv vad du vill ha]
```

---

## Vanliga misstag att undvika

### ❌ Hårdkodad text
```html
<!-- FEL: sajtägaren kan inte ändra denna text -->
<h1>Välkommen till oss</h1>
```

### ✓ Text från content.json
```html
<!-- RÄTT -->
<h1 id="hero-title"></h1>
<!-- sätts sedan med: el.textContent = block.editable.title -->
```

---

### ❌ Hårdkodad bild
```html
<!-- FEL -->
<img src="bilder/hero.jpg" alt="Framsidesbild">
```

### ✓ Bild från content.json
```html
<!-- RÄTT -->
<img id="hero-img" src="" alt="">
<!-- sätts sedan med: img.src = block.editable.image -->
```

---

### ❌ Döpa om fält i content.json efter att adminen är igång
```json
// FEL: "heading" i stället för "title" — adminen slutar fungera rätt
"editable": { "heading": "Välkommen" }
```

### ✓ Behåll fältnamnen stabila
```json
// RÄTT: ändra bara värdet, inte nyckeln
"editable": { "title": "Välkommen" }
```

---

## Lägg till ett nytt blocktyp

1. Lägg till blocket i `content.json`:

```json
{
  "id":    "testimonial",
  "type":  "quote",
  "label": "Kundcitat",
  "editable": {
    "quote":  "Bästa upplevelsen vi haft.",
    "name":   "Anna S.",
    "image":  "bilder/anna.jpg",
    "alt":    "Foto på Anna"
  }
}
```

2. Rendera det i `index.html`:

```js
if (block.type === 'quote') {
  sec.innerHTML = `
    <blockquote>
      <p>"${e.quote ?? ''}"</p>
      <footer>— ${e.name ?? ''}</footer>
    </blockquote>
    ${e.image ? `<img src="${e.image}" alt="${e.alt ?? ''}">` : ''}
  `;
}
```

Adminen hanterar automatiskt alla fält i `editable` utan att du behöver ändra något i admin-koden.

---

## Hantera tomma fält

Sajtägaren kan lämna fält tomma. Rendera defensivt:

```js
// Visa elementet bara om fältet har ett värde
if (e.buttonText) {
  const btn = document.createElement('a');
  btn.href        = e.buttonHref || '#';
  btn.textContent = e.buttonText;
  container.appendChild(btn);
}

// Kortform med nullish coalescing
heading.textContent = e.title ?? '';
```

---

## Checklista innan du är klar

- [ ] All synlig text hämtas från `content.json`
- [ ] Alla synliga bilder hämtas från `content.json`
- [ ] Inga fältnamn i `editable` har bytt namn sedan strukturen sattes
- [ ] Tomma fält kraschar inte sajten
- [ ] `block.id` används konsekvent och matchar i både `content.json` och `index.html`
- [ ] `fetch('./content.json')` fungerar lokalt och på GitHub Pages
- [ ] `admin/`-mappen är orörd

---

## Referens: content.json-strukturen

```
{
  "blocks": [
    {
      "id":       string   ← stabilt, unikt, ändras aldrig
      "type":     string   ← blocktyp, du väljer namn fritt
      "label":    string   ← visas i adminen, kan uppdateras
      "editable": {
        [fältnamn]: string ← fältnamn stabila, värden redigerbara
      }
    }
  ]
}
```

Det är hela kontraktet.

---
---

# Building a website for nano-cms
### A guide for vibe coders — using Claude, Cursor, or other AI tools

---

## What is your role?

You build the **public website** — layout, design, typography, feel. All visual decisions are yours.

**nano-cms** handles the editable content: text and images that the site owner can update from their phone without understanding code.

The only requirement on you is a **contract**: all visible text and all visible images must be fetched from `content.json`. Nothing is hardcoded in HTML.

---

## The files you work with

```
repo/
├── index.html          ← your site (you build this)
├── content.json        ← the content (you define the structure)
├── admin/
│   └── index.html      ← the admin interface (don't touch)
└── bilder/             ← images are uploaded here automatically
```

You own `index.html` and `content.json`. Leave everything inside `admin/` alone.

---

## content.json — the content contract

`content.json` is the interface between you and the site owner. You **define** the structure once. The site owner **edits** values later.

### Basic format

```json
{
  "blocks": [
    {
      "id":    "hero",
      "type":  "imageText",
      "label": "Top section",
      "editable": {
        "title":      "Welcome",
        "text":       "We help you with everything.",
        "image":      "bilder/hero.jpg",
        "alt":        "Front page image",
        "buttonText": "Call us",
        "buttonHref": "tel:+46701234567"
      }
    }
  ]
}
```

### Three rules for fields inside `editable`

1. **Name fields by what they are**, not where they appear. `"title"` not `"hero-heading"`.
2. **Image fields must be named `image`, `img`, or end with these.** The admin recognises them and shows an image picker.
3. **Long text fields must be named `text`, `body`, `description`, or `quote`.** The admin then shows a multi-line input.

### Field names and what the admin does with them

| Your field name                           | Admin shows            |
|-------------------------------------------|------------------------|
| `title`, `heading`, `subtitle`            | Text input (single line) |
| `text`, `body`, `description`, `quote`    | Text input (multi-line) |
| `image`, `img`, `photo`, `*image`, `*img` | Image picker + preview |
| `alt`                                     | Text input (image description) |
| `buttonText`, `linkText`                  | Text input (single line) |
| `buttonHref`, `href`, `url`, `*url`       | URL input              |
| `email`                                   | Email input            |
| `tel`, `phone`                            | Phone input            |
| Everything else                           | Text input (single line) |

---

## Fetching content in index.html

Load `content.json` with `fetch` on page load. Everything must be rendered dynamically.

```html
<div id="site"></div>

<script>
  fetch('./content.json')
    .then(r => r.json())
    .then(data => renderSite(data));

  function renderSite(data) {
    data.blocks.forEach(block => {
      const e = block.editable;
      // Render each block based on block.id or block.type
    });
  }
</script>
```

### Render a specific block by id

```js
function renderSite(data) {
  const hero    = data.blocks.find(b => b.id === 'hero');
  const about   = data.blocks.find(b => b.id === 'about');
  const contact = data.blocks.find(b => b.id === 'contact');

  document.getElementById('hero-title').textContent = hero.editable.title;
  document.getElementById('hero-text').textContent  = hero.editable.text;
  document.getElementById('hero-img').src           = hero.editable.image;
  document.getElementById('hero-img').alt           = hero.editable.alt;
  document.getElementById('hero-btn').textContent   = hero.editable.buttonText;
  document.getElementById('hero-btn').href          = hero.editable.buttonHref;
  // etc.
}
```

### Render all blocks dynamically (type-based)

```js
function renderSite(data) {
  const site = document.getElementById('site');

  data.blocks.forEach(block => {
    const e   = block.editable;
    const sec = document.createElement('section');
    sec.id    = block.id;

    if (block.type === 'imageText') {
      sec.innerHTML = `
        ${e.image ? `<img src="${e.image}" alt="${e.alt ?? ''}">` : ''}
        <div>
          <h2>${e.title ?? ''}</h2>
          <p>${e.text ?? ''}</p>
          ${e.buttonText ? `<a href="${e.buttonHref ?? '#'}">${e.buttonText}</a>` : ''}
        </div>
      `;
    }

    if (block.type === 'paragraph') {
      sec.innerHTML = `
        <h2>${e.title ?? ''}</h2>
        <p>${e.text ?? ''}</p>
      `;
    }

    if (block.type === 'contact') {
      sec.innerHTML = `
        <h2>${e.title ?? ''}</h2>
        <p>${e.text ?? ''}</p>
        ${e.tel   ? `<a href="tel:${e.tel}">${e.tel}</a>` : ''}
        ${e.email ? `<a href="mailto:${e.email}">${e.email}</a>` : ''}
      `;
    }

    site.appendChild(sec);
  });
}
```

---

## Prompt template for vibe coding with AI

Copy and paste this into Claude, Cursor, or similar when asking for help with the site:

```
I'm building a website that fetches all content from content.json via fetch().
The structure of content.json looks like this:

[paste your content.json here]

Rules I must follow:
- No visible text may be hardcoded in HTML — everything must be read from content.json
- No visible image may be hardcoded — image paths must be read from content.json
- Field names inside "editable" must not be changed — the admin depends on them
- block.id values are stable identifiers — do not rename them

Please help me with: [describe what you want]
```

---

## Common mistakes to avoid

### ❌ Hardcoded text
```html
<!-- WRONG: the site owner cannot change this text -->
<h1>Welcome to us</h1>
```

### ✓ Text from content.json
```html
<!-- RIGHT -->
<h1 id="hero-title"></h1>
<!-- then set with: el.textContent = block.editable.title -->
```

---

### ❌ Hardcoded image
```html
<!-- WRONG -->
<img src="bilder/hero.jpg" alt="Front page image">
```

### ✓ Image from content.json
```html
<!-- RIGHT -->
<img id="hero-img" src="" alt="">
<!-- then set with: img.src = block.editable.image -->
```

---

### ❌ Renaming fields in content.json after the admin is live
```json
// WRONG: "heading" instead of "title" — the admin breaks
"editable": { "heading": "Welcome" }
```

### ✓ Keep field names stable
```json
// RIGHT: change the value, not the key
"editable": { "title": "Welcome" }
```

---

## Adding a new block type

1. Add the block to `content.json`:

```json
{
  "id":    "testimonial",
  "type":  "quote",
  "label": "Customer quote",
  "editable": {
    "quote":  "Best experience we've ever had.",
    "name":   "Anna S.",
    "image":  "bilder/anna.jpg",
    "alt":    "Photo of Anna"
  }
}
```

2. Render it in `index.html`:

```js
if (block.type === 'quote') {
  sec.innerHTML = `
    <blockquote>
      <p>"${e.quote ?? ''}"</p>
      <footer>— ${e.name ?? ''}</footer>
    </blockquote>
    ${e.image ? `<img src="${e.image}" alt="${e.alt ?? ''}">` : ''}
  `;
}
```

The admin automatically handles all fields in `editable` without any changes to the admin code.

---

## Handling empty fields

The site owner may leave fields empty. Render defensively:

```js
// Only show the element if the field has a value
if (e.buttonText) {
  const btn = document.createElement('a');
  btn.href        = e.buttonHref || '#';
  btn.textContent = e.buttonText;
  container.appendChild(btn);
}

// Shorthand with nullish coalescing
heading.textContent = e.title ?? '';
```

---

## Checklist before you're done

- [ ] All visible text is fetched from `content.json`
- [ ] All visible images are fetched from `content.json`
- [ ] No field names inside `editable` have been renamed since the structure was set
- [ ] Empty fields don't crash the site
- [ ] `block.id` is used consistently and matches in both `content.json` and `index.html`
- [ ] `fetch('./content.json')` works locally and on GitHub Pages
- [ ] The `admin/` folder is untouched

---

## Reference: the content.json structure

```
{
  "blocks": [
    {
      "id":       string   ← stable, unique, never changes
      "type":     string   ← block type, you choose the name freely
      "label":    string   ← shown in the admin, can be updated
      "editable": {
        [fieldName]: string ← field names stable, values editable
      }
    }
  ]
}
```

That is the entire contract.
