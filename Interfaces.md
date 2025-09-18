# <h2 id="interfaces">⚙️ Interfaces</h2>

Et **interface** i C# er en kontrakt, der **definerer hvilke metoder og properties en klasse skal implementere**, uden at indeholde nogen implementering selv.  
Det bruges ofte til at skabe **løst koblede, testbare og udskiftelige komponenter** i applikationen.


#### Typisk brug
```csharp
// Interface
public interface IActorService
{
    Task<List<ActorDto>> GetAllAsync();
    Task<ActorDto> GetByIdAsync(int id);
    Task CreateAsync(ActorDto actor);
}

// Implementation
public class ActorService : IActorService
{
    private readonly IGenericRepository<Actor> _repo;

    public ActorService(IGenericRepository<Actor> repo)
    {
        _repo = repo;
    }

    public async Task<List<ActorDto>> GetAllAsync()
    {
        var actors = await _repo.GetAllAsync();
        return actors.Select(a => new ActorDto { Id = a.Id, FullName = a.FirstName + " " + a.LastName }).ToList();
    }

    // Andre metoder implementeres her
}

```

...

## Fordele ved Interfaces

- Adskiller **kontrakt fra implementering**  
- Gør koden **mere fleksibel og vedligeholdelsesvenlig**  
- Understøtter **Dependency Injection** og Unit Testing  
- Hjælper med at holde **Application-laget løst koblet** fra Infrastructure-laget  
- **Reducerer stavefejl** i metodekald, da compiler tjekker interfacet  
- Kan fungere som **overenskomst i teams**, fordi alle implementeringer følger samme kontrakt
