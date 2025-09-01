# <h2 id="dtos">⚙️ Dtos</h2>

# 📦 DTOs (Data Transfer Objects)

## Hvad er en DTO?

En **DTO** er et objekt, der bruges til at **overføre data mellem lag i applikationen**, typisk mellem:

- **Frontend (Angular App)** og **API/Controller**  
- **Controller** og **Application-lag (Services)**

Formålet med DTO’er er at **adskille interne Domain-modeller fra de data, der sendes til klienten**.

---

## Hvorfor bruge DTO’er?

1. **Sikkerhed**  
   - Skjuler interne felter i dine Domain-modeller, som klienten ikke skal se.  
   - Eksempel: Password eller interne ID’er.

2. **Enklere dataformater**  
   - DTO’er kan sammensætte data fra flere modeller.  
   - Eksempel: `ActorDto` kan have `FullName` i stedet for separate `FirstName` og `LastName`.

3. **Reduceret dataoverførsel**  
   - Sender kun de felter, der er nødvendige, over netværket.

4. **Decoupling**  
   - DTO’er holder frontend uafhængig af ændringer i Domain-modellerne.

---

## Typisk brug

```csharp
// DTO
public class ActorDto
{
    public int Id { get; set; }
    public string FullName { get; set; }
    public string BirthDate { get; set; }
}

// Controller
[HttpGet]
public async Task<ActionResult<List<ActorDto>>> GetActors()
{
    var actors = await _actorService.GetAllAsync();
    // Mapper Domain-modeller til DTO’er
    return Ok(_mapper.Map<List<ActorDto>>(actors));
}
```
