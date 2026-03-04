# MySql

## program.cs

```
builder.Services.AddDbContext<TestDbContext>(options =>
    options.UseMySql(
        builder.Configuration.GetConnectionString("Default"),
        new MySqlServerVersion(new Version(8, 0, 33)),
        mySqlOptions =>
        {
            mySqlOptions.EnableRetryOnFailure();
        }
    ));
```

## NuGet pakker

```
dotnet add package Pomelo.EntityFrameworkCore.MySql
```

# User Secrets

Når du bruger **User Secrets**, skal din `appsettings.json` faktisk være næsten “tom” for den sensitive del.

Her er den korrekte og professionelle måde at gøre det på i .NET.

---

# ✅ 1️⃣ Sådan skal din `appsettings.json` se ud

Du beholder strukturen – men uden credentials:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": ""
  },

  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },

  "AllowedHosts": "*"
}
```

Eller endnu bedre:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": null
  }
}
```

👉 Pointen er:

- Nøglen skal eksistere
- Men værdien skal ikke indeholde noget følsomt

---

# ✅ 2️⃣ Sådan sætter du User Secrets

I projektmappen:

```bash
dotnet user-secrets init
```

Så:

```bash
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "Server=xxx;Database=xxx;User=xxx;Password=xxx;"
```

Det gemmes nu i:

```
%APPDATA%\Microsoft\UserSecrets\<GUID>\secrets.json
```

Det ligger:

- ✅ Udenfor projektet
- ✅ Udenfor Git
- ✅ Kun på din maskine

---

# ✅ 3️⃣ Skal Program.cs ændres?

Nej 🙂

Hvis du allerede har:

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseMySql(
        builder.Configuration.GetConnectionString("DefaultConnection"),
        ServerVersion.AutoDetect(
            builder.Configuration.GetConnectionString("DefaultConnection")
        )
    ));
```

Så virker det automatisk.

.NET loader i denne rækkefølge:

1. appsettings.json
2. appsettings.Development.json
3. User Secrets (kun i Development)
4. Environment Variables

User Secrets overskriver altså din tomme værdi.

---

# 🔎 Hvis du vil teste at det virker

Tilføj midlertidigt:

```csharp
Console.WriteLine(builder.Configuration.GetConnectionString("DefaultConnection"));
```

Du vil se connection stringen i console – selvom den ikke står i appsettings.

(Slet den igen bagefter 😄)

---

# 💡 Professionel struktur (sådan gør man i enterprise)

| Miljø       | Hvor ligger connection string? |
| ----------- | ------------------------------ |
| Development | User Secrets                   |
| Staging     | Environment Variables          |
| Production  | Environment Variables          |

Aldrig i appsettings i production.

---

Hvis du vil, kan jeg også vise dig forskellen på:

- `appsettings.json`
- `appsettings.Development.json`
- `User Secrets`
- Environment Variables

Så du får 100% styr på config pipeline i .NET 🔥

# Deploying til produktion

Når du deployer dit API til Simply.com, **virker User Secrets ikke**.
De findes kun på din lokale maskine.

I production skal du bruge **Environment Variables på serveren**.

---

# 🎯 Sådan gør du på Simply.com

## ✅ 1️⃣ Lad din appsettings.json være uden credentials

```json
{
  "ConnectionStrings": {
    "DefaultConnection": null
  }
}
```

Ingen passwords i projektet. Punktum.

---

## ✅ 2️⃣ Gå ind i Simply kontrolpanel

På dit webhotel:

- Gå til dit site
- Find **Miljøvariabler / Environment Variables**
- Opret en ny variabel

Navn:

```
ConnectionStrings__DefaultConnection
```

Værdi:

```
Server=xxx;Database=xxx;User=xxx;Password=xxx;
```

⚠️ Husk:
`:` i JSON bliver til `__` (double underscore)

---

## ✅ 3️⃣ Genstart appen

Når du har sat environment variablen:

- Genstart sitet
- Eller genstart App Pool (hvis relevant)

Nu vil .NET automatisk læse den.

---

# 🔎 Hvordan ved jeg det virker?

Du kan midlertidigt teste:

```csharp
var cs = builder.Configuration.GetConnectionString("DefaultConnection");
Console.WriteLine(cs);
```

Hvis den printer din connection string → det virker.

(Slet bagefter!)

---

# 🔐 Hvad sker der teknisk?

I production loader .NET:

1. appsettings.json
2. appsettings.Production.json
3. Environment Variables (overskriver alt)

User Secrets bliver **ikke** loadet udenfor Development.

---

# 🏗 Din arkitektur burde nu være:

### 💻 Lokalt

User Secrets

### 🌍 Production på Simply

Environment Variables

### 📦 Git repo

Ingen credentials

---

# 🔥 Bonus – Hvis du vil gøre det endnu mere robust

Du kan splitte dem op i flere variabler på serveren:

```
DB_SERVER
DB_NAME
DB_USER
DB_PASSWORD
```

Og bygge connection string i `Program.cs`.

Så kan du ændre password uden at ændre hele connection string.

---

Hvis du vil, kan jeg også vise dig:

- Hvordan du tjekker hvilket miljø din app kører i (Development vs Production)
- Eller hvordan du sætter `ASPNETCORE_ENVIRONMENT=Production` korrekt på Simply

Det er næste niveau 😎
