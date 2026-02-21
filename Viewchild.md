`@ViewChild` i **Angular** bruges til at få adgang til et element, en komponent eller en directive direkte fra din template i din TypeScript-klasse.

Det betyder, at du kan “pege” på noget i dit HTML og styre det programmatisk fra din komponent.

---

## 🔹 Hvad er `@ViewChild`?

`@ViewChild` er en decorator, der giver dig en reference til:

* Et DOM-element
* En child component
* En directive

Den bruges typisk sådan her:

```ts
@ViewChild('myInput') myInput!: ElementRef;
```

---

# 1️⃣ Eksempel: Få adgang til et HTML-element

### HTML

```html
<input #myInput type="text">
<button (click)="focusInput()">Focus</button>
```

### TypeScript

```ts
import { Component, ViewChild, ElementRef, AfterViewInit } from '@angular/core';

@Component({
  selector: 'app-example',
  templateUrl: './example.component.html'
})
export class ExampleComponent implements AfterViewInit {

  @ViewChild('myInput') myInput!: ElementRef;

  ngAfterViewInit() {
    console.log(this.myInput);
  }

  focusInput() {
    this.myInput.nativeElement.focus();
  }
}
```

### 🔎 Hvad sker der?

* `#myInput` er en template reference variable
* `@ViewChild('myInput')` finder den
* `ElementRef` giver adgang til det faktiske DOM-element
* Vi kan nu kalde `.focus()` på inputfeltet

---

# 2️⃣ Eksempel: Få adgang til en child component

### Child component

```ts
@Component({
  selector: 'app-child',
  template: `<p>Child works!</p>`
})
export class ChildComponent {
  sayHello() {
    console.log('Hello from child');
  }
}
```

### Parent HTML

```html
<app-child></app-child>
<button (click)="callChild()">Call Child</button>
```

### Parent TypeScript

```ts
@ViewChild(ChildComponent) child!: ChildComponent;

callChild() {
  this.child.sayHello();
}
```

👉 Her får parent direkte adgang til child-komponentens metoder.

---

# 3️⃣ Hvornår er værdien tilgængelig?

`@ViewChild` er **først klar efter viewet er initialiseret**.

Derfor bruges det typisk i:

```ts
ngAfterViewInit()
```

Ikke i constructor og ikke sikkert i `ngOnInit()`.

---

# 4️⃣ `static: true` vs `static: false`

```ts
@ViewChild('myInput', { static: false })
```

### `static: false` (standard)

* Bruges når elementet er inde i `*ngIf`, `*ngFor`
* Først tilgængelig i `ngAfterViewInit`

### `static: true`

* Bruges hvis elementet altid findes
* Tilgængelig allerede i `ngOnInit`

I moderne Angular behøver man sjældent angive det manuelt.

---

# 5️⃣ Hvornår bør man bruge `@ViewChild`?

✅ Når du:

* Skal styre fokus
* Skal integrere tredjepartsbiblioteker
* Skal kalde metode på child component
* Skal måle elementers størrelse

❌ Undgå det hvis:

* Det kan løses med `@Input()` / `@Output()`
* Det bryder komponent-arkitektur

---

# 6️⃣ `@ViewChild` vs `@ViewChildren`

* `@ViewChild` → Én instans
* `@ViewChildren` → Liste (QueryList)

Eksempel:

```ts
@ViewChildren(ChildComponent) children!: QueryList<ChildComponent>;
```

---

# 🎯 Kort fortalt

`@ViewChild` = Direkte reference til noget i din template, så du kan styre det fra din TypeScript-kode.

Hvad vil du se næste?
