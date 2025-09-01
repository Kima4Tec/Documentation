# <h1 id="application">🛠️ Application-lag</h1>

# ⚙️ Application Layer (ProjectName.Application)

## Hvad er Application-laget?

Application-laget definerer applikationens **brugsscenarier og forretningslogik**, uden at kende detaljer om databaser, UI eller andre frameworks. Det fungerer som et mellemled mellem:

- **API/Controller** (håndterer HTTP-requests)  
- **Domain-modeller** (rene entiteter som `Actor` og `Movie`)  
- **Infrastructure-laget** (repositories, databaser, eksterne services)

## Hvad indeholder Application-laget?

1. **Services**  
   - Her ligger logikken for hvad applikationen kan gøre, fx `ActorService` eller `MovieService`.  
   - Servicens **interface** (`IActorService`) definerer kontrakten.  
   - Implementeringen (`ActorService`) håndterer **hvordan** logikken udføres, typisk ved at kalde repositories.

2. **DTO’er (Data Transfer Objects)**  
   - DTO’er bruges til at **pakke data fra controlleren til services** og tilbage.  
   - Adskiller interne Domain-modeller fra det, klienten ser.

3. **Interfaces**  
   - Interfaces som `IActorService` gør laget **løst koblet**, testbart og udskifteligt.  
   - Controlleren kender kun interfacet, ikke implementeringen.

4. **Validering og forretningsregler**  
   - Application-laget kan indeholde regler som:  
     - “En skuespiller skal have fornavn og efternavn.”  
     - “En film må ikke have negativ spilletid.”

---

## Flow


- Controller sender data (DTO) til Service  
- Service validerer, bruger Domain-modeller og kalder Repository  
- Repository gemmer eller henter data fra databasen  
- Service returnerer Domain-modeller eller DTO’er tilbage til Controller  
- Controller sender data til klienten  

---

## Fordele ved Application-laget

1. **Separation of Concerns**: Controller har ikke forretningslogik, og repositories kender ikke til HTTP eller frontend.  
2. **Testbarhed**: Services kan testes med mocks af repositories.  
3. **Løs kobling**: Interfacet (`IService`) gør det let at udskifte implementering.  
4. **Genbrug**: Samme Service kan bruges i forskellige frontends (Angular, Blazor, konsol-app).
