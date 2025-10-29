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
