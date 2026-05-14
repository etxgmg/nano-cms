# nano-cms

Ett ultralätt, mobilanpassat CMS för GitHub Pages — utan databas, utan server, utan installationer.

Sajtägaren redigerar allt innehåll från mobilen. Sajtbyggaren har full frihet att designa sajten. Allt hostas gratis på GitHub Pages.

---

## Så här fungerar det

```
content.json   ←  admin/index.html redigerar och publicerar
     ↓
index.html     ←  läser och renderar innehållet publikt
```

1. **Publik sajt** (`index.html`) hämtar `content.json` och renderar alla block.
2. **Admin** (`admin/index.html`) visar redigerbara kort för varje block och publicerar ändringar direkt till GitHub via API.
3. **Autentisering** sker med ett GitHub Personal Access Token (PAT) som sajtägaren anger vid inloggning. Token lagras bara i sessionminnet — försvinner när fliken stängs.

---

## Snabbstart

### 1. Skapa repot

Forka detta repo eller skapa ett nytt och ladda upp filerna:

```
nano-cms/
├── index.html          ← publik sajt (demo-layout ingår)
├── content.json        ← allt redigerbart innehåll
├── admin/
│   └── index.html      ← mobiladminen
├── bilder/             ← bilder hamnar här vid uppladdning
└── .nojekyll
```

### 2. Aktivera GitHub Pages

Gå till **Settings → Pages → Source: Deploy from a branch → main / (root)**.

Sajten publiceras på: `https://etxgmg.github.io/nano-cms/`
Adminen nås på: `https://etxgmg.github.io/nano-cms/admin/`

### 3. Skapa ett Personal Access Token

Gå till: https://github.com/settings/tokens/new?scopes=repo&description=nano-cms

- Välj scopes: **repo** (fullständig åtkomst till privata och publika repos)
- Sätt en giltighetstid (t.ex. 90 dagar eller "No expiration" för en privat sajt)
- Kopiera token — du ser den bara en gång

### 4. Anpassa konfigurationen

I `admin/index.html`, längst upp i `<script>`-blocket:

```js
const CFG = {
  owner:       'ditt-github-användarnamn',
  repo:        'ditt-repo-namn',
  branch:      'main',
  contentFile: 'content.json',
  imagesDir:   'bilder'
};
```

Gör samma ändring i `index.html` om du ändrar repot.

---

## Innehållsmodellen

All text och alla bilder definieras i `content.json`. Sajtägaren kan bara redigera **värden** — inte lägga till, ta bort eller flytta block.

### Blocktyper (ingår i demo)

| Typ          | Fält                                              |
|--------------|---------------------------------------------------|
| `imageText`  | title, text, image, alt, buttonText, buttonHref   |
| `paragraph`  | title, text                                       |
| `contact`    | title, text, email, tel, address                  |

### Fälttyper (automatisk detektering i adminen)

| Fältnamn             | Visas som         |
|----------------------|-------------------|
| `text`, `body`, etc. | Flerlinjig text   |
| `image`, `img`       | Bildväljare       |
| `alt`                | Textfält          |
| `email`              | E-postfält        |
| `tel`, `phone`       | Telefonfält       |
| `href`, `url`        | URL-fält          |
| Övriga               | Textfält          |

---

## Bygga en ny sajt ovanpå nano-cms

Sajtbyggaren får göra **vad som helst** med layout och design, med ett krav:
> **All synlig text och alla bilder måste läsas från `content.json` — ingenting hårdkodas i HTML.**

Lägg till egna block i `content.json` och rendera dem i `index.html`. Adminen hanterar automatiskt alla fält i `editable`-objektet oavsett blocktyp.

---

## Publiceringsflöde

När sajtägaren trycker "Publicera ändringar" sker följande i bakgrunden:

1. Eventuella nya bilder laddas upp till `bilder/` i repot
2. Bildreferenserna uppdateras i `content.json`
3. `content.json` committas till repot
4. GitHub Pages publicerar automatiskt (tar ~30–60 sekunder)

---

## Begränsningar (version 1)

- Sajtägaren kan **inte** skapa, ta bort eller flytta block
- Sajtägaren kan **inte** ändra layout, design eller tekniska inställningar
- Autentisering kräver ett GitHub PAT — inte OAuth-flöde med backend
- Max filstorlek per bild: ~50 MB (GitHub API-begränsning)

---

## Licens

MIT — fri att använda, modifiera och distribuera.
