# Angular Moderne Struktur

```plaintext
src/
├── app/
│   ├── core/                # Core services, interceptors, auth, guards
│   │   ├── services/
│   │   ├── interceptors/
│   │   ├── guards/
│   │   └── core.config.ts   # Global config (fx provideHttpClient)
│   │
│   ├── features/            # Feature-baserede moduler (standalone components)
│   │   ├── products/
│   │   │   ├── pages/       # Sider/containers (fx ProductListPage)
│   │   │   ├── components/  # Mindre komponenter (fx ProductCard)
│   │   │   ├── services/    # Domænespecifik service
│   │   │   └── product.routes.ts
│   │   │
│   │   └── users/
│   │       ├── pages/
│   │       ├── components/
│   │       ├── services/
│   │       └── user.routes.ts
│   │
│   ├── shared/              # Genbrugelige komponenter, pipes, directives
│   │   ├── components/
│   │   ├── directives/
│   │   └── pipes/
│   │
│   ├── app.routes.ts        # Root routing
│   └── app.component.ts     # Root component (standalone)
│
├── assets/                  # Billeder, ikoner, JSON m.m.
├── environments/            # environment.ts + environment.prod.ts
├── index.html
├── main.ts                  # Bootstrapper app.component
└── styles.css / scss        # Global styling
```


## Core
core/ indeholder globale ressourcer, som kun skal defineres ét sted i hele app’en.
Det er ikke feature-specifikt, men mere fundamentet for hele applikationen.

Typisk indeholder den:

---

### 1. Services

Globale services, som kan bruges på tværs af features.
Eksempler:

- AuthService (login, tokenhåndtering, brugerinfo)

- ApiService (fælles API-kald)

- StorageService (localStorage/sessionStorage)

- LoggerService

Disse services er normalt providedIn: 'root', så de ikke skal importeres manuelt.

---

### 2. Interceptors

HTTP-interceptors, der fanger alle kald via HttpClient.
Eksempler:

- AuthInterceptor (tilføjer JWT-token til headers)

- ErrorInterceptor (global fejlbehandling)

- LoadingInterceptor (viser spinner ved API-kald)

Lægges i core/interceptors/ og registreres i core.config.ts med provideHttpClient.

---

### 3. Guards

Angular Router guards, som styrer adgang til routes.
Eksempler:

- auth.guard.ts → tjekker om brugeren er logget ind

- admin.guard.ts → tjekker om brugeren har admin-rolle

Disse bruges direkte i app.routes.ts eller feature routes.

---

## Features
features/ indeholder alt det, der udgør forretningslogikken i din app.
Hvor core/ er fundamentet (globale ting) og shared/ er genbrug, så er features/ de egentlige "moduler" (selvom vi ikke bruger Angular-moduler længere, men standalone-komponenter).

Man organiserer dem typisk domæne-for-domaene → fx products/, users/, orders/.


### Typisk struktur for en feature

```plaintext
features/
└── products/
    ├── pages/         # Store sider/containers (routable components)
    │   ├── product-list.page.ts
    │   └── product-detail.page.ts
    │
    ├── components/    # Mindre, genbrugelige UI-stykker
    │   ├── product-card.component.ts
    │   └── product-form.component.ts
    │
    ├── services/      # Feature-specifik logik
    │   └── product.service.ts
    │
    └── product.routes.ts  # Feature routing
```

### 1. Pages

- Også kaldet "smart components" eller "container components".

- Routable → man navigerer til dem via Angular Router.

- Henter data (fx via product.service.ts) og sender det videre til components.

- Har oftest minimal UI, men styrer datastrømmen.

**Eksempel:**
```bash
@Component({
  selector: 'app-product-list',
  standalone: true,
  imports: [CommonModule, ProductCardComponent],
  template: `
    <h1>Products</h1>
    <app-product-card *ngFor="let p of products()" [product]="p"></app-product-card>
  `
})
export class ProductListPage {
  private productService = inject(ProductService);
  products = signal<Product[]>([]);

  constructor() {
    this.productService.getAll().subscribe(res => this.products.set(res));
  }
}

```

