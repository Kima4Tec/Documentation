# Lagstruktur
### Lagene og deres ansvar

### 1. Api (præsentationslaget / WebAPI-projektet)
Det eneste lag der ved noget om HTTP og Web.  
Her hører følgende hjemme:

- ✅ **Controllers**
- ✅ **Filters** (f.eks. `ActionFilter`, `ExceptionFilter`)
- ✅ **Model binders**

👉 *Tænk på dette lag som et interface mod brugeren eller klienten.*

---

### 2. Domain (forretningslaget / core logic)
Hjertet af din applikation – *domænemodeller og regler*.  
Her hører følgende hjemme:

- ✅ **Entities / Models**
- ✅ **Value Objects**
- ✅ **Domain Services** (f.eks. regler der ikke naturligt hører til én entitet)
- ✅ **Enums**, **Exceptions**, **Business Rules**

💡 *Det skal være 100% uafhængigt af data/adgang og frameworks.*

---

### 3. Infrastructure-lag
Alt det, der har med lagring og dataadgang at gøre.  
Her hører følgende hjemme:

- ✅ **DbContext**
- ✅ **EF Core konfigurationer** (`OnModelCreating`, `EntityTypeConfiguration`)
- ✅ **Repository-implementeringer**
- ✅ **Migreringer** (valgfrit – kan også være i Api)
- ❌ **Ingen domænelogik eller DTO-håndtering**

---

### 4. Application-lag (ofte mellem Domain og Api)
Bruges typisk i Clean Architecture / DDD for at adskille *use cases* fra præsentation og domain.

- ✅ **Services** (som orchestration/brugsscenarier, fx `CreateOrderService`)
- ✅ **DTO'er**
- ✅ **Input validation attributes** (evt. FluentValidation + integration her)
- 🔁 Mapper DTO’er til domain-objekter og omvendt (AutoMapper eller manuelt)
- ✅ **Interfaces til services**
- 🚫 **Ingen EF Core / DbContext**

*Hvis du ikke har et Application-lag endnu, kan du godt lægge services i Api eller oprette det senere.*

---

### Hvor skal hvad ligge?

| Komponent         | Typisk placering               | Forklaring                      |
|------------------|--------------------------------|----------------------------------|
| `BookController` | `Api/Controllers`              | Web API-kald                     |
| `BookDto`        | `Api/DTOs`                     | Data til/fra klient              |
| `IBookService`   | `Domain/Interfaces`            | Kontrakt for logik               |
| `BookService`    | `Data/Services` (eller `Application`) | Implementering            |
| `IBookRepository`| `Domain/Interfaces`            | Abstraktion for dataadgang       |
| `BookRepository` | `Data/Repositories`            | EF Core-implementering           |
| `Book`           | `Domain/Entities`              | Domænemodel                      |
| `AppDbContext`   | `Data`                         | DbContext for EF Core            |
| `Validation`     | `Api/Validation` (eller `Application`) | FluentValidation eller custom logic |
| `ActionFilter`   | `Api/Filters`                  | Fx logging, exceptions           |

---
