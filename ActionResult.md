# ActionResult
## 🔹 Hvad er `ActionResult`?

`ActionResult` er en **abstrakt baseklasse** i ASP.NET Core, som repræsenterer et HTTP-svar.

Den gør det muligt at returnere:

* ✅ 200 OK
* ❌ 400 Bad Request
* ⛔ 401 Unauthorized
* 🚫 404 Not Found
* 💥 500 Internal Server Error
* * JSON-data

Namespace:

```csharp
using Microsoft.AspNetCore.Mvc;
```

---

## 🔹 Simpelt eksempel

```csharp
[HttpGet("{id}")]
public ActionResult GetUser(int id)
{
    var user = _repository.GetUser(id);

    if (user == null)
        return NotFound();

    return Ok(user);
}
```

Her kan metoden returnere:

* `NotFound()` → 404
* `Ok(user)` → 200 + JSON

Begge arver fra `ActionResult`.

---

## 🔹 Hvorfor ikke bare returnere et objekt?

Du *kan* skrive:

```csharp
public User GetUser(int id)
```

Men så kan du **ikke nemt returnere forskellige statuskoder**.

Med `ActionResult` kan du returnere:

* Data
* Fejl
* Redirect
* Validation errors

Det giver fleksibilitet.

---

# 🔹 `ActionResult<T>` (Bedste praksis i Web API)

I moderne ASP.NET Core API’er bruger man typisk:

```csharp
public ActionResult<User> GetUser(int id)
```

Eksempel:

```csharp
[HttpGet("{id}")]
public ActionResult<User> GetUser(int id)
{
    var user = _repository.GetUser(id);

    if (user == null)
        return NotFound();

    return user; // automatisk 200 OK
}
```

### Fordele:

* ✔️ Strongly typed response
* ✔️ Swagger/OpenAPI forstår returtypen
* ✔️ Mindre kode
* ✔️ Mere clean

---

# 🔹 Almindelige return-typer i Controller

| Metode           | HTTP status |
| ---------------- | ----------- |
| `Ok()`           | 200         |
| `Created()`      | 201         |
| `NoContent()`    | 204         |
| `BadRequest()`   | 400         |
| `Unauthorized()` | 401         |
| `Forbid()`       | 403         |
| `NotFound()`     | 404         |

Eksempel POST:

```csharp
[HttpPost]
public ActionResult<User> CreateUser(CreateUserDto dto)
{
    var user = _service.Create(dto);

    return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
}
```

Returnerer:

* 201 Created
* Location header
* Objektet

---

# 🔹 Forskellen på `IActionResult` og `ActionResult`

| Type              | Hvornår bruges den    |
| ----------------- | --------------------- |
| `IActionResult`   | Interface – fleksibel |
| `ActionResult`    | Baseklasse            |
| `ActionResult<T>` | Bedst til API’er      |

👉 I moderne .NET API’er anbefales **`ActionResult<T>`**

---

# 🔹 I forhold til dit login/JWT-setup

Når du laver login endpoint:

```csharp
[HttpPost("login")]
public ActionResult<LoginResponseDto> Login(LoginDto dto)
{
    var result = _authService.Login(dto);

    if (!result.Success)
        return Unauthorized();

    return Ok(result);
}
```

Her:

* Forkert password → 401
* Korrekt login → 200 + JWT token

---

# 🔥 Kort opsummering

`ActionResult`:

* Repræsenterer et HTTP response
* Giver mulighed for at returnere både data og statuskode
* Brug `ActionResult<T>` i Web API

---

Hvis du vil, kan jeg også forklare:

* 🔹 Hvad der sker internt i pipeline
* 🔹 Forskellen på `Results` i Minimal API vs `ActionResult`
* 🔹 Hvordan model validation automatisk returnerer 400
* 🔹 Hvordan det hænger sammen med Angular HttpClient

Hvad vil du dykke ned i?
