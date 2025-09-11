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

---

### 1. Pages

- Også kaldet "smart components" eller "container components".

- Routable → man navigerer til dem via Angular Router.

- Henter data (fx via product.service.ts) og sender det videre til components.

- Har oftest minimal UI, men styrer datastrømmen.

**Eksempel:**
```ts
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

---

### 2. Components

- Små byggeklodser (dumb components).

- Får data via @Input() og sender events tilbage med @Output().

- Har intet ansvar for datahentning.

**Eksempel:**
```ts
@Component({
  selector: 'app-product-card',
  standalone: true,
  template: `
    <div class="card">
      <h2>{{ product.name }}</h2>
      <p>{{ product.price }} kr</p>
    </div>
  `
})
export class ProductCardComponent {
  @Input() product!: Product;
}

```


---

### 3. Services

- Kun til denne feature → isoleret logik.

- Håndterer API-kald, caching, signaler eller state for products.

**Eksempel:**
```ts
@Injectable({ providedIn: 'root' })
export class ProductService {
  private http = inject(HttpClient);

  getAll(): Observable<Product[]> {
    return this.http.get<Product[]>('/api/products');
  }

  getById(id: number): Observable<Product> {
    return this.http.get<Product>(`/api/products/${id}`);
  }
}


```
---

### 4. Feature routes

- Definerer navigation for denne feature.

- Bruges sammen med lazy loading i root app.routes.ts.

**product.routes.ts**
```ts
import { Routes } from '@angular/router';
import { ProductListPage } from './pages/product-list.page';
import { ProductDetailPage } from './pages/product-detail.page';

export const PRODUCT_ROUTES: Routes = [
  { path: '', component: ProductListPage },
  { path: ':id', component: ProductDetailPage }
];

```


**app.routes.ts**
```ts
import { Routes } from '@angular/router';
import { ProductListPage } from './pages/product-list.page';
import { ProductDetailPage } from './pages/product-detail.page';

export const PRODUCT_ROUTES: Routes = [
  { path: '', component: ProductListPage },
  { path: ':id', component: ProductDetailPage }
];



```

---

#### Eksempel med Products

**Her er inkluderet:**
- ProductListPage (side der viser alle produkter)

- ProductDetailPage (side der viser ét produkt via route param)

- ProductCardComponent (genbrugelig komponent)

- ProductService (API-kald)

- product.routes.ts (lazy loaded routing)

**Struktur**
```bash
src/app/features/products/
├── pages/
│   ├── product-list.page.ts
│   └── product-detail.page.ts
│
├── components/
│   └── product-card.component.ts
│
├── services/
│   └── product.service.ts
│
└── product.routes.ts
```


<img width="752" height="382" alt="image" src="https://github.com/user-attachments/assets/8ef52641-4501-4155-b825-19b4418c46f3" />

