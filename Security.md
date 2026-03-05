# Sikkerhed mod injection fra hackere
Perfekt — her er **5 production-settings til et ASP.NET Core API på IIS**, som mange professionelle APIs bruger. De tager få minutter at sætte op, men forbedrer sikkerheden ret meget. 🔐

---

# 1️⃣ Fjern server-information (fingerprinting)

Angribere scanner ofte efter server-typer.

Standard IIS sender headers som:

```
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
```

Det fortæller hackere præcis hvad du kører.

### Løsning i `web.config`

```xml
<system.webServer>
  <security>
    <requestFiltering removeServerHeader="true" />
  </security>

  <httpProtocol>
    <customHeaders>
      <remove name="X-Powered-By" />
    </customHeaders>
  </httpProtocol>
</system.webServer>
```

Nu ved bots ikke hvad din server er.

---

# 2️⃣ Begræns request size

Bots kan forsøge at sende enorme requests for at crashe API'et.

### I `web.config`

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <requestLimits maxAllowedContentLength="10485760" />
    </requestFiltering>
  </security>
</system.webServer>
```

Her:

```
10485760 = 10 MB
```

Det er rigeligt til JSON APIs.

---

# 3️⃣ Bloker HTTP methods du ikke bruger

Mange angreb bruger mærkelige metoder som:

```
TRACE
OPTIONS
TRACK
```

### I `web.config`

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <verbs allowUnlisted="false">
        <add verb="GET" allowed="true" />
        <add verb="POST" allowed="true" />
        <add verb="PUT" allowed="true" />
        <add verb="DELETE" allowed="true" />
      </verbs>
    </requestFiltering>
  </security>
</system.webServer>
```

Nu kan kun disse metoder bruges.

---

# 4️⃣ Tilføj security headers

Security headers beskytter mod flere webangreb.

```xml
<httpProtocol>
  <customHeaders>
    <add name="X-Content-Type-Options" value="nosniff" />
    <add name="X-Frame-Options" value="DENY" />
    <add name="X-XSS-Protection" value="1; mode=block" />
  </customHeaders>
</httpProtocol>
```

Disse bruges på mange production APIs.

---

# 5️⃣ Slå detaljerede fejl fra

I production må brugere aldrig se stack traces.

I `Program.cs`:

```csharp
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/error");
}
```

Og lav endpoint:

```csharp
app.Map("/error", () =>
{
    return Results.Problem("Der opstod en fejl.");
});
```

Nu ser brugeren kun en generisk fejl.

---

# 🧠 Ekstra ting jeg også vil anbefale til dit API

### Rate limiting (.NET 7+)

```csharp
builder.Services.AddRateLimiter(options =>
{
    options.AddFixedWindowLimiter("api", opt =>
    {
        opt.PermitLimit = 100;
        opt.Window = TimeSpan.FromMinutes(1);
    });
});
```

---

### CORS (ved frontend)

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("frontend",
        policy => policy
            .WithOrigins("https://ditfrontend.dk")
            .AllowAnyHeader()
            .AllowAnyMethod());
});
```

---
Fedt – her er **de 7 mest almindelige angreb mod et ASP.NET Core API**, og hvordan du lukker dem. Hvis du implementerer dem, er dit API allerede **ret professionelt sikret**. 🔐

---

# 1️⃣ SQL Injection

Angriber forsøger at ændre SQL ved at sende manipuleret input.

### Eksempel på angreb

```
' OR 1=1 --
```

### Sådan undgår du det

Brug **Entity Framework Core** LINQ queries:

```csharp
var user = await _context.Users
    .FirstOrDefaultAsync(x => x.Username == username);
```

Undgå:

```csharp
var sql = $"SELECT * FROM Users WHERE Username = '{username}'";
```

Hvis du bruger raw SQL:

```csharp
.FromSqlInterpolated($"SELECT * FROM Users WHERE Username = {username}")
```

---

# 2️⃣ Mass Assignment (meget almindeligt i APIs)

Angriber sender ekstra felter i JSON.

### Eksempel

```json
{
 "username": "test",
 "password": "123",
 "role": "admin"
}
```

Hvis du direkte mapper til database model kan angriberen blive admin.

### Løsning: DTO

```csharp
public class CreateUserDto
{
    public string Username { get; set; }
    public string Password { get; set; }
}
```

Controller:

```csharp
public async Task<IActionResult> Create(CreateUserDto dto)
```

---

# 3️⃣ Ubeskyttet API (ingen auth)

Som du selv opdagede via Postman.

Alle kan kalde endpoints.

### Løsning

Brug:

* JWT authentication
* eller API key

Eksempel:

```csharp
[Authorize]
public class PersonController : ControllerBase
```

---

# 4️⃣ Overposting / ID manipulation

Angriber ændrer ID i request.

### Eksempel

```
PUT /api/users/1
```

Body:

```json
{
 "id": 2
}
```

Kan ændre en anden bruger.

### Løsning

Ignorer ID fra body:

```csharp
if (id != model.Id)
{
    return BadRequest();
}
```

Eller brug DTO uden ID.

---

# 5️⃣ Manglende input validation

Angriber sender:

* ekstrem lange strings
* scripts
* invalid data

### Løsning

Brug attributes:

```csharp
public class CreatePersonDto
{
    [Required]
    [MaxLength(50)]
    public string Name { get; set; }
}
```

ASP.NET Core returnerer automatisk:

```
400 Bad Request
```

---

# 6️⃣ Rate attack / API spam

Bots kan sende 1000 requests.

### Løsning: Rate limiting

I .NET:

```csharp
builder.Services.AddRateLimiter(options =>
{
    options.AddFixedWindowLimiter("api", opt =>
    {
        opt.PermitLimit = 100;
        opt.Window = TimeSpan.FromMinutes(1);
    });
});
```

Og:

```csharp
app.UseRateLimiter();
```

---

# 7️⃣ Eksponering af fejl (information leak)

Hvis din API viser interne fejl kan angribere lære dit system.

### Farligt

```
MySqlException: access denied for user
```

### Løsning

Brug global exception handler:

```csharp
app.UseExceptionHandler("/error");
```

---

# 🔒 Ekstra sikkerhed (meget anbefalet)

### CORS

Hvis du kun har frontend ét sted.

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("frontend",
        policy =>
        {
            policy.WithOrigins("https://ditfrontend.dk")
                  .AllowAnyHeader()
                  .AllowAnyMethod();
        });
});
```

---

### HTTPS

Du har allerede:

```csharp
app.UseHttpsRedirection();
```

✔ godt.

---

# 🧠 Det vigtigste for dit API lige nu

Jeg ville prioritere:

1️⃣ API key eller JWT
2️⃣ DTOs i controllers
3️⃣ Validation attributes
4️⃣ Rate limiting

Så er dit API **meget sværere at angribe**.

---


