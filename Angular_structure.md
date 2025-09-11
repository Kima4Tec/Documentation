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
