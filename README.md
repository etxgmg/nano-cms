# nano-cms

**Bygg en liten företagssajt med Claude — gratis hosting, inga månadsavgifter, och företagaren redigerar själv text och bilder från mobilen.**

Passar perfekt för hantverkare, frisörer, konsulter, restauranger och andra småföretag som behöver en enkel webbplats utan att betala för ett dyrt CMS.

---

## Vad är det här?

Ett mallrepo med allt som behövs för att sätta upp en statisk webbplats på GitHub Pages med ett mobilanpassat redigeringsgränssnitt. Du bygger sajten med AI-hjälp. Företagaren uppdaterar sedan text och bilder direkt från sin telefon — ingen kod, inga inloggningar på konstiga system, inga tekniska kunskaper krävs.

```
Det du bygger:          Det företagaren får:

index.html         →    En snygg publik sajt
content.json       →    Allt redigerbart innehåll
admin/index.html   →    En mobilapp-känsla för att
                        redigera text och byta bilder
```

**Hosting:** GitHub Pages — helt gratis.
**Redigering:** Direkt från mobilen på `sajt.github.io/admin/`.
**Publicering:** Tryck på en knapp. Klart.

---

## För dig som ska bygga sajten med AI (vajbkoda)

> *Du behöver: ett GitHub-konto åt klienten, Claude eller liknande AI-verktyg, och ca 30 minuter.*

### Steg 1 — Förbered

1. Skapa ett GitHub-konto åt klienten (eller använd ett befintligt)
2. Skapa ett nytt tomt repo på klientens konto
3. Skapa ett Personal Access Token med `repo`-scope på klientens konto:
   `github.com/settings/tokens/new?scopes=repo`

### Steg 2 — Instruera din AI

Öppna [`AI-INSTRUCTIONS.md`](./AI-INSTRUCTIONS.md) i det här repot. Kopiera hela innehållet. Fyll i:

```
Klientens GitHub-användarnamn : [användarnamn]
Klientens repo-namn           : [repo-namn]
Klientens GitHub-nyckel (PAT) : [ghp_...]
```

Skriv en kort sajt-brief längst ned — bransch, känsla, sektioner, färger. Ge sedan allt till din AI och be den följa instruktionerna.

### Steg 3 — Aktivera GitHub Pages

På klientens repo: **Settings → Pages → Source: main / (root)**

Klart. Sajten är live på `https://[klientens-användarnamn].github.io/[repo-namn]/`
och adminen på `.../admin/`.

### Bygg du sajten manuellt istället?

Läs [`SITE-BUILDER-GUIDE.md`](./SITE-BUILDER-GUIDE.md) — den förklarar kontraktet och visar med kodexempel exakt vad din `index.html` måste göra.

---

## För företagaren — så här redigerar du din sajt

1. Gå till `https://[din-sajt].github.io/[repo-namn]/admin/` i din mobiltelefon
2. Logga in med din GitHub-nyckel (du får den av den som byggt sajten)
3. Redigera text och byt bilder direkt i korten som visas
4. Tryck **Publicera ändringar** — sajten uppdateras inom en minut

Det är allt. Inga ord som JSON, commit eller branch dyker upp.

---

## Vad kan företagaren redigera?

Allt som syns på sajten: rubriker, brödtext, knapptexter, kontaktuppgifter, bilder. Det enda företagaren *inte* kan göra är att ändra layout och design — det kräver att sajtbyggaren gör en uppdatering.

---

## Teknisk översikt

| Del | Teknologi |
|-----|-----------|
| Hosting | GitHub Pages (gratis) |
| Publik sajt | Vanilla HTML/CSS/JS, ingen byggprocess |
| Innehåll | `content.json` — hämtas vid sidladdning |
| Admin | Single-page HTML-app, skriver till GitHub via API |
| Autentisering | GitHub Personal Access Token, lagras i sessionsminnet |
| Bilder | Laddas upp till `bilder/` i repot via GitHub API |

---

## Filer i det här mallrepot

| Fil | Vad den är |
|-----|------------|
| `AI-INSTRUCTIONS.md` | Ge den till din AI — beskriver hela uppdraget |
| `SITE-BUILDER-GUIDE.md` | Kontraktsregler och kodexempel för sajtbyggaren |
| `admin/index.html` | Mobiladminen — kopieras och konfigureras per klient |
| `content.json` | Exempel på innehållsstruktur |
| `index.html` | Exempelsajt som visar hur innehållet renderas |

---

## Begränsningar

- Företagaren kan inte skapa, ta bort eller flytta block — bara redigera text och bilder
- Inloggning kräver ett GitHub PAT — inget magiskt lösenord att komma ihåg, men ett extra steg vid setup
- Passar statiska sajter — inte webbutiker med kundvagn eller inloggade användare

---
---

# nano-cms

**Build a small business website with Claude — free hosting, no monthly fees, and the business owner edits text and images from their phone.**

Perfect for tradespeople, hairdressers, consultants, restaurants, and other small businesses that need a simple website without paying for an expensive CMS.

---

## What is this?

A template repo with everything needed to set up a static website on GitHub Pages with a mobile-friendly editing interface. You build the site with AI assistance. The business owner then updates text and images directly from their phone — no code, no logins to strange systems, no technical knowledge required.

```
What you build:         What the business owner gets:

index.html         →    A good-looking public site
content.json       →    All editable content
admin/index.html   →    An app-like interface for editing
                        text and swapping images
```

**Hosting:** GitHub Pages — completely free.
**Editing:** Directly from a phone at `site.github.io/admin/`.
**Publishing:** Press a button. Done.

---

## For site builders using AI (vibe coding)

> *You need: a GitHub account for the client, Claude or a similar AI tool, and about 30 minutes.*

### Step 1 — Set up

1. Create a GitHub account for the client (or use an existing one)
2. Create a new empty repo on the client's account
3. Create a Personal Access Token with `repo` scope on the client's account:
   `github.com/settings/tokens/new?scopes=repo`

### Step 2 — Instruct your AI

Open [`AI-INSTRUCTIONS.md`](./AI-INSTRUCTIONS.md) in this repo. Copy the entire contents. Fill in:

```
Client's GitHub username : [username]
Client's repo name       : [repo-name]
Client's GitHub PAT      : [ghp_...]
```

Write a short site brief at the bottom — industry, feel, sections, colours. Hand everything to your AI and ask it to follow the instructions.

### Step 3 — Enable GitHub Pages

On the client's repo: **Settings → Pages → Source: main / (root)**

Done. The site is live at `https://[client-username].github.io/[repo-name]/`
and the admin at `.../admin/`.

### Building the site manually instead?

Read [`SITE-BUILDER-GUIDE.md`](./SITE-BUILDER-GUIDE.md) — it explains the contract and shows with code examples exactly what your `index.html` must do.

---

## For the business owner — how to edit your site

1. Go to `https://[your-site].github.io/[repo-name]/admin/` on your phone
2. Log in with your GitHub key (provided by whoever built your site)
3. Edit text and swap images directly in the cards shown on screen
4. Press **Publish changes** — the site updates within a minute

That's all. Words like JSON, commit, or branch never appear.

---

## What can the business owner edit?

Everything visible on the site: headings, body text, button labels, contact details, images. The only thing the business owner *cannot* do is change layout and design — that requires the site builder to make an update.

---

## Technical overview

| Part | Technology |
|------|------------|
| Hosting | GitHub Pages (free) |
| Public site | Vanilla HTML/CSS/JS, no build step |
| Content | `content.json` — fetched on page load |
| Admin | Single-page HTML app, writes to GitHub via API |
| Authentication | GitHub Personal Access Token, stored in session memory |
| Images | Uploaded to `bilder/` in the repo via GitHub API |

---

## Files in this template repo

| File | What it is |
|------|------------|
| `AI-INSTRUCTIONS.md` | Hand this to your AI — describes the full assignment |
| `SITE-BUILDER-GUIDE.md` | Contract rules and code examples for the site builder |
| `admin/index.html` | The mobile admin — copied and configured per client |
| `content.json` | Example content structure |
| `index.html` | Example site showing how content is rendered |

---

## Limitations

- The business owner cannot create, delete, or reorder blocks — only edit text and images
- Login requires a GitHub PAT — not a magic password to memorise, but an extra setup step
- Suited for static sites — not webshops with shopping carts or logged-in users
