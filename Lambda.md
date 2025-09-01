<h1 id="lambda">Lambda</h1>

## Lambda Expressions

En **lambda expression** er en kort måde at definere en **anonym funktion** på.  
Den bruges ofte sammen med LINQ, events eller delegates.

**Syntax:**
```csharp
(parameters) => expression

```

**Eksempel:**
```csharp
List<int> numbers = new List<int> { 1, 2, 3, 4, 5 };

// Find alle tal større end 3
var result = numbers.Where(n => n > 3).ToList();
// result: [4, 5]
```
**Forklaring:**

- numbers er en liste af heltal (List<int>).

- .Where(...) er en LINQ-metode, der filtrerer elementer i listen baseret på en betingelse (predicate).

- n => n > 3 er en lambda expression, som definerer betingelsen:

    - n repræsenterer hvert element i listen

    - n > 3 returnerer true for de elementer, vi vil beholde

- Resultatet af numbers.Where(n => n > 3) er en ny liste med kun de tal, som opfylder betingelsen: [4, 5].

- .ToList() konverterer resultatet fra en IEnumerable<int> til en faktisk List<int>.
