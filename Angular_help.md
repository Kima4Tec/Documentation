# Angular help

# Indholdsfortegnelse
1. [App Routes](#approutes)
2. [APp](#app)

# AppRoutes
## Old routes (klassisk)

- **Kendetegn:**
    - Du importerer alle komponenter øverst i filen.   
    - Hver route har en component, der bliver hentet med det samme, når appen starter.   
    - Det betyder: jo flere komponenter du importerer, jo større bliver din initiale bundle size (det, browseren skal hente ved opstart).   

- **Fordele:**   
    - Simpelt og direkte.   
    - Passer godt til små projekter eller hvis alt indhold skal være tilgængeligt med det samme.   

- **Ulemper:**   
    - Dårlig performance ved mange sider, fordi alt bliver indlæst upfront.   
    - Ikke optimalt til større apps med mange moduler/sider.   

```bash
import { Routes } from '@angular/router';
import { LandingpageComponent } from './landingpage/landingpage.component';
import { ProfilpageComponent } from './profilpage/profilpage.component';
import { ContactpageComponent } from './contactpage/contactpage.component';
import { AdminpageComponent } from './adminpage/adminpage.component';
import { RegisterComponent } from './registerpage/registerpage.component';
import { LoginComponent } from './loginpage/loginpage.component';
import { AuthGuard } from './auth.guard';


export const routes: Routes = [
    { path: '', component: LandingpageComponent },
    { path: 'profil', component: ProfilpageComponent },
    { path: 'contact', component: ContactpageComponent },
    { path: 'admin', component: AdminpageComponent, canActivate: [AuthGuard] },
    { path: 'register', component: RegisterComponent, canActivate: [AuthGuard] },
    { path: 'login', component: LoginComponent },
    { path: '', redirectTo: '/login', pathMatch: 'full' },
];
```

## New Routes (lazy loading af standalone komponenter)
- **Kendetegn:**
    - Du importerer ikke komponenterne direkte.
    - I stedet bruger du loadComponent, som dynamisk loader komponenten kun når brugeren navigerer til den route.
    - Dette er lazy loading – altså at Angular først henter koden for siden, når den faktisk skal bruges.
    - Denne metode kræver, at dine komponenter er standalone (standalone: true).

- **Fordele:*

✅ Hurtigere initial load – kun nødvendige filer hentes.  
✅ Mindre bundle size → bedre performance.  
✅ Meget nemt at opsætte lazy loading uden ekstra moduler.  
✅ Matcher den moderne Angular-arkitektur (Angular 17+ bruger dette som standard).  

- **Ulemper:**

⚠️ Kræver at dine komponenter er standalone.  
⚠️ Kan være lidt sværere at teste i ældre setup med moduler.  


```bash
import { Routes } from '@angular/router';

export const routes: Routes = [
  { path: '', redirectTo: '/author', pathMatch: 'full' },
  {
    path: 'author',
    loadComponent: () => import('./pages/author').then((m) => m.AuthorPage),
  },
  {
    path: 'artist',
    loadComponent: () => import('./pages/artist').then((m) => m.ArtistPage),
  },
  {
    path: 'cover',
    loadComponent: () => import('./pages/cover').then((m) => m.CoverPage),
  },
  {
    path: 'book',
    loadComponent: () => import('./pages/book').then((m) => m.BookPage),
  },
];
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
