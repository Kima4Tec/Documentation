# 🚀 Arrow Functions Cheatsheet

Arrow functions (`=>`) er en kortere måde at skrive funktioner på i JavaScript/TypeScript.

---

## 🔹 Grundlæggende syntaks

### Uden parametre
```ts
const hello = () => console.log("Hej!");
```

### Én parameter (uden parenteser)
```ts
const square = x => x * x;
```

### Flere parametre
```ts
const multiply = (a, b) => a * b;
```

### Med block body (flere linjer kode)
```ts
const greet = (name) => {
  console.log("Hej " + name);
  return "Velkommen!";
};
```

---

## 🔹 `this` forskel

### Normal function
```ts
function Timer() {
  this.seconds = 0;

  setInterval(function() {
    this.seconds++; // ❌ 'this' refererer ikke til Timer
  }, 1000);
}
```

### Arrow function (arver `this`)
```ts
function Timer() {
  this.seconds = 0;

  setInterval(() => {
    this.seconds++; // ✅ 'this' kommer fra Timer
  }, 1000);
}
```

---

## 🔹 Inline brug (meget almindeligt)
```ts
const numbers = [1, 2, 3, 4];
const doubled = numbers.map(n => n * 2);
// [2, 4, 6, 8]
```

---

## 🔹 I Angular (lazy loading)
```ts
{
  path: 'home',
  loadComponent: () =>
    import('./pages/landing-page/landing-page')
      .then(m => m.LandingPage),
}
```
👉 Her bruges arrow function + dynamic import til **lazy loading** af en komponent.

---

## ✅ Hvornår skal du bruge arrow functions?
- Når du vil have **kortere kode**
- Når du vil beholde `this` fra det ydre scope (callbacks, async, events)
- Når du arbejder med array-metoder (`map`, `filter`, `reduce` osv.)

