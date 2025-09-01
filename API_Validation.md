# 🛡️ Validering i .NET API og Angular App

Validering kan foregå både på **server-side** (.NET) og **client-side** (Angular).  
Ofte kombineres begge for at sikre **god brugeroplevelse** og **datasikkerhed**.

---

## 🔹 1. Data Annotations (server-side)

Indbygget i .NET – du bruger attributter direkte på modellen.

```csharp
public class ApprenticeDto
{
    [Required(ErrorMessage = "Navn er påkrævet")]
    [StringLength(100, ErrorMessage = "Navnet må maks være 100 tegn")]
    public string Name { get; set; }

    [Range(18, 65, ErrorMessage = "Alder skal være mellem 18 og 65")]
    public int Age { get; set; }
}
```

**Fordele:**
- Nem at komme i gang med (bygget ind i .NET).
- God til simple regler (fx påkrævet, max længde, range).
- Automatisk understøttet af ASP.NET Core Model Validation.

**Ulemper:**
- Mindre fleksibel til komplekse regler (fx afhængigheder mellem felter).
- Reglerne er koblet direkte til modellen.

---

## 🔹 2. FluentValidation (server-side)

Eksternt bibliotek hvor regler defineres i validators i stedet for attributter.

```csharp
using FluentValidation;

public class ApprenticeDtoValidator : AbstractValidator<ApprenticeDto>
{
    public ApprenticeDtoValidator()
    {
        RuleFor(x => x.Name)
            .NotEmpty().WithMessage("Navn er påkrævet")
            .MaximumLength(100).WithMessage("Navnet må maks være 100 tegn");

        RuleFor(x => x.Age)
            .InclusiveBetween(18, 65).WithMessage("Alder skal være mellem 18 og 65");
    }
}
```

**Opsætning i `Program.cs`:**

```csharp
builder.Services.AddValidatorsFromAssemblyContaining<ApprenticeDtoValidator>();
```

**Fordele:**
- Meget fleksibel (betinget logik, komplekse regler).
- Holder modeller rene for attributter.
- God integration med ASP.NET Core.

**Ulemper:**
- Ekstra afhængighed (ekstern pakke).
- Lidt mere opsætning end Data Annotations.

---

## 🔹 3. Validering i Angular (client-side)

Giver brugeren hurtig feedback via formularer.  
Kan være **Template-driven** eller **Reactive Forms** (mest udbredt).

```ts
this.form = this.fb.group({
  name: ['', [Validators.required, Validators.maxLength(100)]],
  age: ['', [Validators.required, Validators.min(18), Validators.max(65)]]
});
```

**HTML:**

```html
<input formControlName="name" />
<div *ngIf="form.controls['name'].hasError('required')">
  Navn er påkrævet
</div>
```

**Fordele:**
- Øjeblikkelig feedback til brugeren.
- God brugeroplevelse.
- Nem visning af fejlbeskeder i UI.

**Ulemper:**
- **Kan ikke stå alene**: Brugeren kan manipulere requesten → server-side validering er stadig nødvendig.

---

## 🔹 Bedste praksis

👉 Brug **begge lag**:
1. **Client-side (Angular)** → For god UX og hurtig feedback.  
2. **Server-side (.NET)** → For datasikkerhed og integritet (Data Annotations eller FluentValidation).  

- Start småt med **Data Annotations**.  
- Skift til **FluentValidation** hvis reglerne bliver komplekse.  

---

## 🔹 Sammenligning

|                 | Data Annotations | FluentValidation | Angular |
|-----------------|------------------|------------------|---------|
| **Hvor**        | Server (.NET)    | Server (.NET)    | Client (browser) |
| **Sværhedsgrad**| Let              | Middel           | Middel |
| **Fleksibilitet**| Lav             | Høj              | Middel |
| **Feedback til bruger** | Nej (kræver roundtrip) | Nej (kræver roundtrip) | Ja (øjeblikkeligt) |
| **Sikkerhed**   | Høj (server-side) | Høj (server-side) | Lav (kan manipuleres) |

---

## 🔹 Flow i praksis

- Angular: Validerer formen → viser fejlbeskeder lokalt.  
- .NET API: Validerer requesten igen med Data Annotations eller FluentValidation.  
- Hvis valideringen fejler på serveren → returnér en `400 Bad Request` med fejlbeskeder, som Angular kan vise.
