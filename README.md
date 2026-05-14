# nano-cms

Ett ultralätt, mobilanpassat CMS för GitHub Pages — utan databas, utan server, utan installationer.

Det här är ett **mallrepo**. Det används av en konsult för att snabbt sätta upp en redigerbar webbplats åt en klient. Klienten får en sajt på sin egen GitHub som de kan uppdatera text och bilder på från mobilen — utan något tekniskt kunnande.

---

## Tre roller

| Roll | Ansvar |
|------|--------|
| **Konsulten** | Sätter upp klientens GitHub-konto och repo, skriver sajt-briefen, instruerar AI:n |
| **AI:n (sajtbyggaren)** | Läser instruktionerna, bygger `index.html` och `content.json`, driftsätter på klientens repo |
| **Klienten (sajtägaren)** | Redigerar text och bilder från mobilen via adminens webbgränssnitt — behöver inte förstå kod |

---

## Hur det fungerar

```
etxgmg/nano-cms          klientens repo
(detta mallrepo)   →     ├── index.html       ← publik sajt
                         ├── content.json     ← allt redigerbart innehåll
                         ├── admin/
                         │   └── index.html   ← mobiladmin
                         └── bilder/
```

1. **Publik sajt** — `index.html` hämtar `content.json` och renderar hela sidan. All text och alla bilder kommer från JSON-filen, ingenting är hårdkodat.
2. **Mobiladmin** — `admin/index.html` visar redigerbara kort för varje textblock och bild. Klienten loggar in med sin GitHub-nyckel, redigerar, trycker "Publicera".
3. **Publicering** — adminen skriver direkt till klientens GitHub-repo via API. GitHub Pages publicerar automatiskt inom ~60 sekunder.

---

## Konsultens arbetsflöde per klient

### 1. Sätt upp klientens GitHub

- Skapa ett GitHub-konto åt klienten (eller använd ett befintligt)
- Skapa ett nytt tomt repo på klientens konto
- Skapa ett Personal Access Token med `repo`-scope: `github.com/settings/tokens`

### 2. Instruera AI:n

Kopiera `AI-INSTRUCTIONS.md` från detta repo. Fyll i:

```
Klientens GitHub-användarnamn : [användarnamn]
Klientens repo-namn           : [repo-namn]
Klientens GitHub-nyckel (PAT) : [ghp_...]
```

Skriv en sajt-brief längst ned i filen och ge den till din AI (t.ex. Claude, Cursor).

### 3. AI:n levererar

AI:n läser kontraktsreglerna från detta repo, bygger en sajt anpassad för klientens brief och driftsätter filerna direkt på klientens GitHub. Den kopierar adminens kod och pekar om den till klientens repo.

### 4. Aktivera GitHub Pages

På klientens repo: **Settings → Pages → Source: main / (root)**

Sajten är live på: `https://[klientens-användarnamn].github.io/[repo-namn]/`
Adminen nås på: `https://[klientens-användarnamn].github.io/[repo-namn]/admin/`

---

## Filer i detta mallrepo

| Fil | Syfte |
|-----|-------|
| `AI-INSTRUCTIONS.md` | Instruktionsfil som konsulten fyller i och ger till AI:n |
| `SITE-BUILDER-GUIDE.md` | Kontraktsregler för AI:n som bygger sajten (sv + en) |
| `admin/index.html` | Mobiladmin — kopieras och konfigureras per klient |
| `content.json` | Exempelstruktur för innehållsmodellen |
| `index.html` | Exempelsajt som visar hur content.json renderas |

---

## Innehållsmodellen

All synlig text och alla bilder definieras i `content.json`. Klienten kan bara redigera **värden** — aldrig lägga till, ta bort eller flytta block.

### Exempelstruktur

```json
{
  "blocks": [
    {
      "id":    "hero",
      "type":  "imageText",
      "label": "Översta delen",
      "editable": {
        "title":      "Välkommen",
        "text":       "Vi hjälper dig.",
        "image":      "bilder/hero.jpg",
        "alt":        "Framsidesbild",
        "buttonText": "Ring oss",
        "buttonHref": "tel:+46701234567"
      }
    }
  ]
}
```

### Automatisk fältdetektering i adminen

| Fältnamn | Visas som |
|----------|-----------|
| `text`, `body`, `description` | Flerlinjig text |
| `image`, `img`, `photo` | Bildväljare + förhandsvisning |
| `alt` | Textfält (bildbeskrivning) |
| `email` | E-postfält |
| `tel`, `phone` | Telefonfält |
| `href`, `url`, `buttonHref` | URL-fält |
| Övriga | Textfält (enrad) |

---

## Begränsningar (version 1)

- Klienten kan **inte** skapa, ta bort eller flytta block — bara redigera värden
- Klienten kan **inte** ändra layout, design eller tekniska inställningar
- Inloggning i adminen kräver ett GitHub PAT — inget OAuth-flöde med backend
- Bilder laddas upp direkt via GitHub API (max ~50 MB per fil)

---

## Licens

MIT — fri att använda, modifiera och distribuera.
