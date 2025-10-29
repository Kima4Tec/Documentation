# React og Next JS

**create app** 
- ingen store bogstaver

```
npx create-next-app@latest first-react
cd first-react
npm run dev
```

ved npm run dev, får man besked om, at der er brug for administrator adgang pga node.js og firewall

**og går man så ind på localhost:3000 ser man en status 200 i cmd:**

```bash
C:\Users\km\source\repos\first-react>npm run dev

> first-react@0.1.0 dev
> next dev

   ▲ Next.js 16.0.1 (Turbopack)
   - Local:        http://localhost:3000
   - Network:      http://10.0.12.129:3000

 ✓ Starting...
 ✓ Ready in 827ms
 GET / 200 in 2.0s (compile: 1891ms, render: 145ms)
```

Tailwind bliver installeret som standard!

### Første side og visning af denne
```bash
app/
├─ page.tsx         ← Hovedsiden, tilgås via "/"
├─ about/
│  └─ page.tsx     ← Din About-side


export default function About() {
    return (<div className="text-4xl">Om mig</div>)
}

URL: http://localhost:3000/about
```

Brug denne for lokal kørsel i modsætning til server.
```
"use client";
```

---

## Komponenten
```
const [data, setData] = useState<WeatherForecast[]>([]);

```
Vi bruger array destructuring fra JavaScript. Med `[data, setData]` giver vi navnene til de to elementer.


### useState
useState er en React hook, der bruges til at gemme lokal state i en komponent.   
**Den returnerer et array med to elementer:**
- Selve state-værdien (data)
- En funktion til at opdatere state (setData)

| Del                           | Forklaring                                                                       |
| ----------------------------- | -------------------------------------------------------------------------------- |
| `useState<WeatherForecast[]>` | Siger til TypeScript, at `data` skal være et array af WeatherForecast            |
| `[]` (i parentes)             | Den **initiale værdi** for state = tomt array                                    |
| `[data, setData]`             | Array-destructuring: `data` = værdien, `setData` = funktion til at ændre værdien |

