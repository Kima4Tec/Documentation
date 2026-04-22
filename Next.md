# `next` i Angular / RxJS

I Angular arbejder vi ofte med **Observables**, fx når vi henter data fra en service, lytter til events eller håndterer async data.

`next` er en nøgle-del af `Observable.subscribe()`.

---

## 🔹 Hvad er `next`?

* `next` er **callback-funktionen**, der kaldes **hver gang Observable udsender en værdi**.
* Den håndterer altså selve dataen, når den kommer.

Eksempel med HTTP-request:

```ts
this.http.get<User[]>('/api/users').subscribe({
  next: (users) => {
    console.log(users); // Håndter data her
  },
  error: (err) => {
    console.error(err); // Håndter fejl
  },
  complete: () => {
    console.log('Request færdig');
  }
});
```

---

## 🔹 Hvorfor bruge `next`?

1. **Håndtere data fra async kald**
   Når data kommer fra server, WebSocket, form eller event.

2. **Struktureret subscribe**
   Moderne Angular/TypeScript anbefaler:

   ```ts
   .subscribe({
     next: ...,
     error: ...,
     complete: ...
   });
   ```

   i stedet for den gamle:

   ```ts
   .subscribe(data => { ... });
   ```

   Fordel: tydelig opdeling af data, fejl og completion.

3. **Sammen med Subject / BehaviorSubject**
   Du kan også **sende** nye værdier ud med `next`:

   ```ts
   this.currentUserSubject.next(loggedInUser);
   ```

   * Alle subscribers får besked om den nye værdi
   * Brugt fx i login-systemer, state management mv.

---

## 🔹 Kort oversigt

| Metode       | Hvad den gør                   |
| ------------ | ------------------------------ |
| `next()`     | Sender eller modtager en værdi |
| `error()`    | Håndterer fejl                 |
| `complete()` | Kører når Observable er færdig |

---

## 🔹 Brugseksempel med service

```ts
// person.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Person } from '../models/person.model';

@Injectable({ providedIn: 'root' })
export class PersonService {
  constructor(private http: HttpClient) {}

  getPeople(): Observable<Person[]> {
    return this.http.get<Person[]>('/api/persons');
  }
}

// persons.component.ts
this.personService.getPeople().subscribe({
  next: (people) => {
    this.people = people; // Gem data i komponenten
  },
  error: (err) => console.error(err)
});
```

---

✅ **Kort sagt:**

* Brug `next` når du vil håndtere data, som en Observable sender.
* Brug den også med `Subject` for at sende nye værdier til subscribers.

---

