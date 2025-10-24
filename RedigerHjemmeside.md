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

# 4. Hybrid løsning: Angular + Headless CMS

Mange moderne sider (også større danske sites) bruger denne kombination:

- Angular frontend for hastighed og designkontrol.

- Strapi eller Sanity som headless CMS for let redigering.

---


# 5 Løsninger hvis du vil bruge databaser med GitHub Pages

## 1. Ekstern API   
- Host API et andet sted (f.eks. Simply.com, Render, Azure)

- API’en håndterer MySQL / SQLite

- GitHub Pages frontend kalder API via HTTP (GET, POST etc.)

## 2. Serverless / cloud-løsning

- Firebase Firestore, Supabase, eller Airtable

- Kan bruges direkte fra frontend JS på GitHub Pages

## 3. Bare statisk

Hvis du kun har HTML/JS uden behov for database, kan du gemme alt i JSON-filer og hente med fetch fra GitHub Pages. Man kan sagtens bruge SVG-billeder og gemme dem i JSON.


| Platform               | Popularitet                         | Bedst til                                    |
| ---------------------- | ----------------------------------- | -------------------------------------------- |
| **Firebase Firestore** | Meget populær, stort community      | Real-time apps, chat, små/medium projekter   |
| **Supabase**           | Stadig voksende, open-source        | SQL-baserede apps, relationer, fleksibilitet |
| **Airtable**           | Mest populær til content management | CMS-lignende projekter med lav kompleksitet  |

