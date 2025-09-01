<h1 id="linq">LINQ</h1>

**LINQ** er en måde at skrive spørgsmål/filtrering, sortering og projektion af data i C# på en læsbar måde.

**Eksempel med lambda:**
```csharp
var actors = new List<Actor>
{
    new Actor { FirstName = "Tom", LastName = "Hanks" },
    new Actor { FirstName = "Meryl", LastName = "Streep" },
    new Actor { FirstName = "Leonardo", LastName = "DiCaprio" }
};

// Find alle skuespillere hvor fornavn starter med 'M'
var mActors = actors.Where(a => a.FirstName.StartsWith("M")).ToList();

```

**Eksempel med LINQ query syntax:**
```csharp
var mActorsQuery = from a in actors
                   where a.FirstName.StartsWith("M")
                   select a;

```
#### Fordele

- Kort og læsbar kode

- Integrerer direkte med collections og databaser (via Entity Framework)

- Reducerer behov for loops og manuel filtrering


### Typiske LINQ-metoder

| Metode              | Beskrivelse                           | Eksempel                                           |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
| `Where`             | Filter elementer baseret på betingelse | `numbers.Where(n => n > 3)`                       |
| `Select`            | Projektion / transformation           | `actors.Select(a => a.FullName)`                  |
| `OrderBy`           | Sortering stigende                    | `numbers.OrderBy(n => n)`                         |
| `OrderByDescending` | Sortering faldende                    | `numbers.OrderByDescending(n => n)`               |
| `FirstOrDefault`    | Hent første element eller null        | `actors.FirstOrDefault(a => a.FirstName == "Tom")`|
| `Any`               | Tjekker om nogen opfylder betingelse | `numbers.Any(n => n > 10)`                        |

