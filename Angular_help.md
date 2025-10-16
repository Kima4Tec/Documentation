# Angular help

# Indholdsfortegnelse
1. [App Routes](#approutes)
2. [APp](#app)

# AppRoutes
## Old routes

## New Routes

## Local routers med referencer
Her er der oprettet routes i **admin.routes.ts**, og der er lavet refs til denne i **app.routes.ts**
```bash
import { Routes } from '@angular/router';
import { adminRoutes } from './features/admin/admin.routes';

export const routes: Routes = [
  ...adminRoutes, // Tilføjer alle admin routes her
  { path: '', redirectTo: 'admin/datacontrol', pathMatch: 'full' },
  { path: '**', redirectTo: 'admin/datacontrol' },
];
```
