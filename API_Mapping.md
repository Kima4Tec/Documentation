# <h2 id="mappings">⚙️ Mappings</h2>
# 🔄 Mappings

## Hvad er Mappings?

**Mapping** er processen, hvor man **oversætter data mellem forskellige objekttyper** i applikationen.  
I et typisk .NET API bruges mapping til at konvertere mellem:

- **Domain-modeller** (f.eks. `Actor`, `Movie`)  
- **DTO’er** (f.eks. `ActorDto`, `MovieDto`)  
- Eventuelt **ViewModels** eller andre interne objekter  

Formålet er at **adskille interne strukturer fra data, der sendes til klienten**, og samtidig kunne tilføje ekstra transformationer.

---

## Fordele ved mapping

1. **Adskillelse af concerns**  
   - Domain-modeller indeholder alle felter og forretningslogik  
   - DTO’er sender kun de data, klienten har brug for

2. **Reducerer fejl og gentagelser**  
   - Mapper automatisk felter mellem objekter i stedet for at skrive `dto.Id = entity.Id;` manuelt  

3. **Understøtter fleksible API’er**  
   - Kan sammensætte data fra flere modeller til én DTO  
   - Kan ændre feltformater, fx `FullName = FirstName + " " + LastName`

4. **Lettere test og vedligeholdelse**  
   - Mapper håndteres ét sted, hvilket gør det lettere at opdatere, hvis Domain-modeller ændres

---

## Typisk brug i vores API

Vi bruger ofte **AutoMapper**, som automatisk kan mappe mellem Domain-modeller og DTO’er.

### Opsætning af AutoMapper

```csharp
public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Actor, ActorDto>()
            .ForMember(dest => dest.FullName,
                       opt => opt.MapFrom(src => src.FirstName + " " + src.LastName));
        CreateMap<ActorDto, Actor>();
    }
}
```
