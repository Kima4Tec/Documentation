# React komponenter
## Header
```jsx
"use client";
import React from "react";

export default function Header() {
  return (
    <header className="bg-blue-600 text-white p-4 w-full flex justify-between items-center">
      <div className="flex flex-row items-baseline gap-10">
        <div className="text-2xl font-bold">Finans Dashboard</div>
        <div className="text-md">Aktiekurser</div>
        <div className="text-md">Økonomi</div>
        <div className="text-md">Investering</div>
      </div>
      <div>Login</div>
    </header>
  );
}

```

## Sidebar
```jsx
"use client";
import React from "react";

interface LinkItem {
  label: string;
  href: string;
}

const links: LinkItem[] = [
  { label: "Morningstar", href: "https://www.morningstar.com/" },
  { label: "Yahoo Finance", href: "https://finance.yahoo.com/" },
  { label: "MSN Finance", href: "https://www.msn.com/en-us/money" },
  { label: "Finance Charts", href: "https://www.tradingview.com/charts/" },
  { label: "Investing.com", href: "https://www.investing.com/" },
];

export default function Sidebar() {
  return (
    <aside className="w-60 bg-gray-800 text-white p-4 shrink-0">
      <h2 className="text-xl font-bold mb-4">Links</h2>
      <ul className="space-y-2">
        {links.map((link) => (
          <li key={link.href}>
            <a
              href={link.href}
              target="_blank"
              rel="noopener noreferrer"
              className="hover:text-yellow-400"
            >
              {link.label}
            </a>
          </li>
        ))}
      </ul>
    </aside>
  );
}

```
