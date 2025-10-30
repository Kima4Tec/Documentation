# Core Concepts

 
### 1. SPA 
- vi modificere indholdet af en side og giver den nyt indhold. Vi opdaterer alle komponenter i DOM

### 2. Components 
- Den visuelle del af vores lag, der splitter vores UI i små dele. Fx Header, Navigation, Sidebar. Det er en **class** eller **function**, der returnerer HTML. Det kalder man **JSX** i React.

### 3. JSX (JavaScript XML)
- className i stedet for class
- ```<button onClick={('Hej med dig!)}></button>```
- JSX bliver compileret til HTML og JS før det bliver vist i browseren. 
- I Angular ville det se sådan ud:   
```<button (click)="sayHello()">Hej</button>```

### 4. URL Routing
- I det gamle React brugte man BrowerRouter
- Nu navigerer man med <Link href="/path">, som er Next JS indbyggede routing. 

### 5. Props
- du videregiver data fra en komponenent til en anden med props (properties) -den kan blivebragt som en function parameter. Man kan sende det videre ned i mange lag, og det kalder man prop-drilling. 

### 6. State
- et javascript objekt, brugt til at repræsentere information i eller om en komponent. Det er vist mere normalt at bruge hooks i moderne React
- State er intern data i en komponent.
- Den kan ændres over tid, typisk som resultat af brugerhandlinger (klik, input osv.).
- Når state ændres, re-render komponenten automatisk med den nye værdi.
- Kan ændres med setState (useState)

### 7. Component LifeCycle
- initialization  

**3 phases**
1. mounting - når den bliver tilføjet til DOM
2. updating - komponent opdateres
3. Unmounting - løsnet fra DOM
- **I funktionelle komponenter bruger man i stedet useEffect for at gøre det samme:**

#### 8. Hooks (React 16.8+)
- Hooks er funktioner, der giver funktionelle komponenter adgang til React-funktioner som state, lifecycle osv.
- De starter altid med use, fx: useState, useEffect.
- useState() //--> Set og opdatere state
- useEffect() //--> Udfør sideeffekter i lifecycle
- Der findes en del hooks, og man kan også bygge sine egne.

### 9. State Management
#### Local State
Til enkelte komponenter eller små apps er useState og useReducer stadig standard.

#### Global State

Når flere komponenter skal dele data, kan man bruge:

**React Context**
- Indbygget i React.
- God til mindre globale data, f.eks. tema eller login-status.   

**State management biblioteker**
- Til større apps bruges ofte Redux, Zustand, Jotai, Recoil osv.

- De giver centraliseret state, der kan opdateres fra hvor som helst.

### Virtual DOM
Lær hvordan det virker. React laver en virtual DOM, der er en repræsenterer den reelle DOM. 

### Key Prop
Hvert item skal have et unique id fx:
```<li key={note.id}>```

### EventListeners
onSubmit, OnDrop, Ondrag, OnMouseLeave, Onclick


### Handling forms
#### Controlled Components
Vi sætter en onChange, der styrer lytter til forandring i state
**Eksempel:**
```
import { useState } from "react";

export default function MyForm() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault(); // forhindrer page reload
    console.log("Navn:", name, "Email:", email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Navn:
        <input
          type="text"
          value={name} // state styrer input
          onChange={(e) => setName(e.target.value)} // opdaterer state
        />
      </label>
      <label>
        Email:
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
      </label>
      <button type="submit">Send</button>
    </form>
  );
}
```
**Forklaring:**

- value={name} binder input til state.
- onChange opdaterer state, når brugeren skriver.
- handleSubmit kan bruge state til at sende data til backend eller API

#### Uncontrolled Componenents


### Conditional Rendering
- Man kan bruge if med logiske && operatorer
```
const show = true;
return (
  <div>
    {show && <p>Dette vises kun hvis show er true</p>}
  </div>
);
```
- Man kan bruge if-else med conditional operator
```
const loggedIn = false;

return (
  <div>
    {loggedIn ? <p>Velkommen!</p> : <p>Log ind først</p>}
  </div>
);
```

### Almindelige kommandoer
npx create-react-app <appname>
npm start
npm run build

**extra:**
npm run dev
node server.js