# 1. Headless CMS (mest moderne og fleksibel løsning)

**Et Headless CMS** lader dig have et backend-system, hvor du (eller kunden) kan redigere tekst, billeder, sektioner osv.
Frontend (Angular, React, Vue, eller ren HTML/JS) henter data via API.

### Eksempler:

- Strapi (open source, kører i Node.js)

- Sanity.io

- Contentful

- Directus

- Payload CMS

### Fordele:

Du kan fortsætte med Angular eller ren JS som frontend.

Brugeren redigerer indhold via et admin-panel (uden at røre koden).

Du styrer selv data-struktur og API’er.

God til flersprogethed, billeder, artikler, tekster osv.

### Ulemper:

Du skal sætte et API op (Node.js, Strapi, etc.).

Kræver hosting både af CMS og frontend.

#### 👉 Typisk flow:

   Angular → henter data fra CMS API → viser tekst/billeder.
Når bruger opdaterer tekst i CMS → API’et opdateres → hjemmesiden viser ændringen næste gang.


---


# 2. Database + Adminpanel (bygget selv i .NET eller Node.js)

Du kan lave dit eget mini-CMS:

- Lav en API/backend (f.eks. i .NET Core, Express eller NestJS)

- Gem tekst og billeder i en database (SQLite, MySQL, PostgreSQL osv.)

- Lav en admin-side i Angular, hvor du kan redigere indhold og gemme til databasen.

### Fordele:

- Du har fuld kontrol (alt er dit).

- Kan integreres direkte med resten af appen.

### Ulemper:

- Mere udviklingsarbejde.

- Du skal selv håndtere autentifikation og database.

#### 👉 Eksempel:
Et Content-endpoint med felter som section, title, body, imageUrl.
Frontend kalder f.eks. GET /api/content/about for at vise teksten — og PUT /api/content/about for at ændre den.

---

.

# 3. Brug et klassisk CMS

Hvis du ikke vil bygge alt selv:

- WordPress (PHP)

- Umbraco (.NET)

- Drupal, Joomla, etc.

De giver en “færdig” redigeringsoplevelse med WYSIWYG, men du mister noget fleksibilitet, især hvis du allerede koder i Angular.

---
