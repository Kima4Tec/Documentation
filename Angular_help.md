# Angular help

# Indholdsfortegnelse
1. [App Routes](#approutes)
2. [APp](#app)

# AppRoutes
## Old routes

## New Routes
```bash
  {
    path: 'admin/datacontrol',
    loadComponent: () =>
      import('./pages/data-control-page/data-control-page').then(m => m.DataControlPage),
  },
```

## Local routers med referencer
Her er der oprettet routes i **admin.routes.ts**, og der er lavet refs til denne i **app.routes.ts**

**admin.routes.ts**
```bash
import { Routes } from '@angular/router';

export const adminRoutes: Routes = [
  {
    path: 'admin/datacontrol',
    loadComponent: () =>
      import('./pages/data-control-page/data-control-page').then(m => m.DataControlPage),
  },
  {
    path: 'admin/filmcontrol',
    loadComponent: () =>
      import('./pages/film-control-page/film-control-page').then(m => m.FilmControlPage),
  },
  {
    path: 'admin/usercontrol',
    loadComponent: () =>
      import('./pages/user-control-page/user-control-page').then(m => m.UserControlPage),
  },
];
```


**app.routes.ts**
```bash
import { Routes } from '@angular/router';
import { adminRoutes } from './features/admin/admin.routes';

export const routes: Routes = [
  ...adminRoutes, // Tilføjer alle admin routes her
  { path: '', redirectTo: 'admin/datacontrol', pathMatch: 'full' },
  { path: '**', redirectTo: 'admin/datacontrol' },
];
```
