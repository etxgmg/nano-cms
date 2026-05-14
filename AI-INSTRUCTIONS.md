# nano-cms — instruktioner för AI-sajtbyggare

Det här repot (`etxgmg/nano-cms`) är ett **mallrepo**. Du ska inte bygga något här.

Du ska bygga en sajt åt en **klient** och leverera den till klientens eget GitHub-repo. Det är där slutresultatet hamnar.

---

## Steg 1 — Läs reglerna

Läs dessa två filer innan du skriver en enda rad kod:

- **Kontraktet:** https://raw.githubusercontent.com/etxgmg/nano-cms/main/SITE-BUILDER-GUIDE.md
- **Admin-koden:** https://raw.githubusercontent.com/etxgmg/nano-cms/main/admin/index.html

SITE-BUILDER-GUIDE.md förklarar alla regler. Admin-koden ska du kopiera och anpassa — se steg 3.

---

## Steg 2 — Fyll i klientuppgifter

*Konsulten fyller i detta innan instruktionen lämnas till AI:n.*

```
Klientens GitHub-användarnamn : [FYLL I]
Klientens repo-namn           : [FYLL I]
Klientens GitHub-nyckel (PAT) : [FYLL I – behövs för att publicera via adminen]
```

---

## Steg 3 — Det du ska leverera

Tre filer, pushade till klientens repo:

### 1. `content.json`
Definiera innehållsstrukturen utifrån sajten du ska bygga. Följ formatet från SITE-BUILDER-GUIDE.md. Alla synliga texter och bilder ska ha redigerbara fält. Fyll i rimliga exempeltexter som platshållare — klienten ersätter dem via adminen.

### 2. `index.html`
Den publika sajten. All CSS och JavaScript inline i en enda fil. Hämtar `./content.json` vid sidladdning och renderar hela sidan därifrån. Ingen text och ingen bild hårdkodas. Följ kontraktet i SITE-BUILDER-GUIDE.md.

### 3. `admin/index.html`
Kopiera admin-koden från https://raw.githubusercontent.com/etxgmg/nano-cms/main/admin/index.html

Uppdatera sedan **bara dessa rader** i `<script>`-blocket längst upp:

```js
const CFG = {
  owner:       'KLIENTENS-GITHUB-ANVÄNDARNAMN',  // ← ändra
  repo:        'KLIENTENS-REPO-NAMN',             // ← ändra
  branch:      'main',
  contentFile: 'content.json',
  imagesDir:   'bilder'
};
```

Rör ingenting annat i admin-koden.

---

## Steg 4 — Övriga filer

Pusha även dessa till klientens repo:

- `.nojekyll` — tom fil, krävs för GitHub Pages
- `bilder/.gitkeep` — tom fil, skapar bildkatalogen

---

## Steg 5 — Aktivera GitHub Pages

Aktivera GitHub Pages på klientens repo:
`Settings → Pages → Source: main branch / (root)`

Sajten publiceras på: `https://[klientens-användarnamn].github.io/[repo-namn]/`
Adminen nås på: `https://[klientens-användarnamn].github.io/[repo-namn]/admin/`

---

## Kontraktsregler — icke förhandlingsbara

- Ingen synlig text hårdkodas i HTML
- Ingen synlig bild hårdkodas i HTML
- Fältnamnen i `editable` ändras inte efter att strukturen är satt
- `block.id`-värden är stabila — byt dem aldrig
- Tomma fält kraschar inte sajten — rendera defensivt med `?? ''`
- `admin/index.html` ändras inte, förutom `CFG`-objektet

---

## Checklista innan leverans

- [ ] `content.json` har alla synliga texter och bilder som redigerbara fält
- [ ] `index.html` hämtar och renderar allt från `content.json`
- [ ] `admin/index.html` har korrekt `owner` och `repo` i CFG
- [ ] Tomma fält kraschar inte sajten
- [ ] `.nojekyll` och `bilder/.gitkeep` finns med
- [ ] GitHub Pages är aktiverat på klientens repo

---

## Sajt-brief

*Konsulten beskriver önskad sajt här — look, känsla, bransch, målgrupp, tonalitet. Ju mer konkret desto bättre.*

**Exempel:**
> Rörmokeri i Göteborg. Tre anställda. Vill ha en trygg, professionell känsla — mörkblå och vit. Tydlig CTA "Ring oss" direkt i hero. Sektioner: hero, om oss, tjänster, kontakt. Mobilanpassad.

---
---

# nano-cms — instructions for AI site builders

This repo (`etxgmg/nano-cms`) is a **template repo**. Do not build anything here.

You are building a site for a **client** and delivering it to the client's own GitHub repo. That is where the output goes.

---

## Step 1 — Read the rules

Read these two files before writing a single line of code:

- **The contract:** https://raw.githubusercontent.com/etxgmg/nano-cms/main/SITE-BUILDER-GUIDE.md
- **The admin code:** https://raw.githubusercontent.com/etxgmg/nano-cms/main/admin/index.html

SITE-BUILDER-GUIDE.md explains all the rules. The admin code is to be copied and adapted — see step 3.

---

## Step 2 — Fill in client details

*The consultant fills this in before handing the instructions to the AI.*

```
Client's GitHub username : [FILL IN]
Client's repo name       : [FILL IN]
Client's GitHub PAT      : [FILL IN — needed for publishing via the admin]
```

---

## Step 3 — What you will deliver

Three files, pushed to the client's repo:

### 1. `content.json`
Define the content structure based on the site you are building. Follow the format from SITE-BUILDER-GUIDE.md. All visible text and images must have editable fields. Fill in reasonable placeholder text — the client replaces it via the admin.

### 2. `index.html`
The public site. All CSS and JavaScript inline in a single file. Fetches `./content.json` on page load and renders the entire page from it. No text and no image is hardcoded. Follow the contract in SITE-BUILDER-GUIDE.md.

### 3. `admin/index.html`
Copy the admin code from https://raw.githubusercontent.com/etxgmg/nano-cms/main/admin/index.html

Then update **only these lines** in the `<script>` block at the top:

```js
const CFG = {
  owner:       'CLIENT-GITHUB-USERNAME',  // ← change this
  repo:        'CLIENT-REPO-NAME',        // ← change this
  branch:      'main',
  contentFile: 'content.json',
  imagesDir:   'bilder'
};
```

Do not touch anything else in the admin code.

---

## Step 4 — Additional files

Also push these to the client's repo:

- `.nojekyll` — empty file, required for GitHub Pages
- `bilder/.gitkeep` — empty file, creates the images directory

---

## Step 5 — Enable GitHub Pages

Enable GitHub Pages on the client's repo:
`Settings → Pages → Source: main branch / (root)`

The site publishes at: `https://[client-username].github.io/[repo-name]/`
The admin is at: `https://[client-username].github.io/[repo-name]/admin/`

---

## Contract rules — non-negotiable

- No visible text is hardcoded in HTML
- No visible image is hardcoded in HTML
- Field names in `editable` are not changed after the structure is set
- `block.id` values are stable — never rename them
- Empty fields do not crash the site — render defensively with `?? ''`
- `admin/index.html` is not modified except for the `CFG` object

---

## Checklist before delivery

- [ ] `content.json` has all visible text and images as editable fields
- [ ] `index.html` fetches and renders everything from `content.json`
- [ ] `admin/index.html` has the correct `owner` and `repo` in CFG
- [ ] Empty fields do not crash the site
- [ ] `.nojekyll` and `bilder/.gitkeep` are included
- [ ] GitHub Pages is enabled on the client's repo

---

## Site brief

*The consultant describes the desired site here — look, feel, industry, audience, tone. The more specific the better.*

**Example:**
> Plumbing company in Gothenburg. Three employees. Professional, trustworthy feel — dark blue and white. Clear CTA "Call us" in the hero. Sections: hero, about us, services, contact. Mobile-first.
