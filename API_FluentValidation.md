# 📘 FluentValidation i .NET

FluentValidation er et populært open-source bibliotek til at håndtere **validering af modeller** i .NET-applikationer.  
Det giver mere fleksibilitet og bedre struktur end traditionelle Data Annotations.

---

## 🔹 Installation

Installer via NuGet:

```bash
dotnet add package FluentValidation.AspNetCore
```

---

## 🔹 Opsætning i ASP.NET Core

I `Program.cs` eller `Startup.cs`:

```csharp
using FluentValidation;
using FluentValidation.AspNetCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddValidatorsFromAssemblyContaining<Program>();
builder.Services.AddFluentValidationAutoValidation();
builder.Services.AddFluentValidationClientsideAdapters();

var app = builder.Build();
app.MapControllers();
app.Run();
```

Dette sikrer, at dine validators bliver **registreret automatisk**.

---

## 🔹 Oprettelse af en Validator

I stedet for Data Annotations på modellen, laver du en separat validator.

```csharp
public class ApprenticeDto
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Email { get; set; }
}
```

Validator:

```csharp
using FluentValidation;

public class ApprenticeDtoValidator : AbstractValidator<ApprenticeDto>
{
    public ApprenticeDtoValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Navn er påkrævet")
            .MaximumLength(100).WithMessage("Navn må maks være 100 tegn");

        RuleFor(x => x.Age)
            .InclusiveBetween(18, 65).WithMessage("Alder skal være mellem 18 og 65");

        RuleFor(x => x.Email)
            .NotEmpty().WithMessage("Email er påkrævet")
            .EmailAddress().WithMessage("Ugyldig email");
    }
}
```

---

## 🔹 Avancerede regler

### Betingede regler
```csharp
RuleFor(x => x.Email)
    .NotEmpty()
    .When(x => x.Age > 18)
    .WithMessage("Email kræves for personer over 18 år");
```

### Custom logik
```csharp
RuleFor(x => x.Name)
    .Must(name => name.All(char.IsLetter))
    .WithMessage("Navnet må kun indeholde bogstaver");
```

### Regler for lister/child-objekter
```csharp
RuleForEach(x => x.Courses)
    .SetValidator(new CourseValidator());
```

---

## 🔹 Integration med Controllers

Når du har registreret FluentValidation, sker validering automatisk.  
Eksempel på en controller:

```csharp
[ApiController]
[Route("api/[controller]")]
public class ApprenticeController : ControllerBase
{
    [HttpPost]
    public IActionResult Create(ApprenticeDto apprentice)
    {
        if (!ModelState.IsValid)
            return BadRequest(ModelState);

        return Ok("Apprentice created!");
    }
}
```

Hvis data er ugyldige, returneres automatisk en **400 Bad Request** med fejlbeskeder.

---

## 🔹 Returnering af fejl til Angular

Fejl returneres typisk i JSON-format:

```json
{
  "errors": {
    "Name": [ "Navn er påkrævet" ],
    "Age": [ "Alder skal være mellem 18 og 65" ]
  }
}
```

I Angular kan du vise disse fejl i formularen, fx:

```ts
this.http.post('/api/apprentice', form.value).subscribe({
  error: (err) => {
    this.errors = err.error.errors;
  }
});
```

---

## 🔹 FluentValidation vs Data Annotations

| Funktion                | Data Annotations       | FluentValidation       |
|--------------------------|------------------------|------------------------|
| **Sværhedsgrad**         | Nem                   | Middel                 |
| **Fleksibilitet**        | Begrænset             | Meget høj              |
| **Separering af logik**  | Nej (på modellen)     | Ja (separate klasser)  |
| **Avancerede regler**    | Svært                 | Nemt                   |
| **Brug i ASP.NET Core**  | Indbygget             | Ekstern pakke          |

---

## 🔹 Bedste praksis

- Brug **FluentValidation** når du har komplekse regler eller vil holde modeller rene.  
- Brug **Data Annotations** til simple projekter med få valideringskrav.  
- Kombinér altid med **client-side validering** (fx Angular Reactive Forms) for bedst brugeroplevelse.

---

## 🔹 Ekstra ressourcer

- 📘 Dokumentation: [https://docs.fluentvalidation.net](https://docs.fluentvalidation.net)
- 💻 GitHub repo: [https://github.com/FluentValidation/FluentValidation](https://github.com/FluentValidation/FluentValidation)
