```
using FilmFrame.Application.Interfaces;
using FilmFrame.Infrastructure.Data;
using Microsoft.EntityFrameworkCore;


namespace FilmFrame.Infrastructure.Repositories
{
    /// <summary>
    /// Et generisk repository, der giver mulighed for at udføre CRUD operationer ved at administrere entities af typen <typeparamref name="T"/> 
    /// i den underliggende database.Interfacet <see cref="IGenericRepository{T}"/> definerer de metoder, som dette repository implementerer.
    /// </summary>
    /// <remarks>Med dette repository kan man udføre basale CRUD operationer (Create, Read, Update, Delete) for
    /// typen  <typeparamref name="T"/>. Den bruger en instans af <see cref="ApplicationDbContext"/> for at gemme i databasen.</remarks>
    /// <typeparam name="T">Med "where T : class" begrænser jeg T til at være en klasse, 
    /// altså en reference og ikke en value-type, som fx bool og int.</typeparam>
    public class GenericRepository<T> : IGenericRepository<T> where T : class
    {
        //Depencency Injection af DbContext. Feltet er readonly, da det kun skal sættes i konstruktøren,
        //og ikke skal kunnes ændres efterfølgende. Det er privat, så det kun kan bruges i denne klasse,
        //så ingen udefra kan begynde at ændre direkte i database-konteksten.
        private readonly ApplicationDbContext _context;

        //Konstruktør, der tager en ApplicationDbContext som parameter. _context sættes til parameteren context.
        //På den måde får jeg en instans af DbContext, som alle metoder kan bruge til at interagere med databasen.
        public GenericRepository(ApplicationDbContext context)
        {
            _context = context;
        }

        //Den henter alle poster i databasen af type T. Det gør den asynkront.
        //Posterne hentes asynkront ind i listen List. Task<List> betyder at
        //metoden giver et objekt af typen List. Jeg bruger Task fordi jeg
        //gerne vil kunne returnere den nye instans med opdaterede værdier.
        //Set fortæller, at den skal hente DbSet af typen.Fx kan det være alle
        //poster i _context.Actor, hvis T er Actor.
        public async Task<List<T>> GetAllAsync()
        {
            return await _context.Set<T>().ToListAsync();
        }

        //Den henter en enkelt post i databasen af typen T baseret på dens Id.
        //Det gør den ved FindAsync(Id)-metoden, der er en EF-core metode.Id skal
        //være en primær-nøgle.Den kigger først i DbContext cache, det man kalder
        //Change Tracker, og hvis data ikke findes her, så laver den en SQL-request.
        //Hvis posten er tom altså null, så skal der kastes en exception. KeyNotFoundException
        //er en indbygget exception, som man bruger, når man ikke kan finde en nøgle, fx en
        //primærnøgle. typeof(T).Name giver navnet på typen. Task<T> betyder at metoden returnerer
        //en instans med opdaterede værdier.
        public async Task<T> GetByIdAsync(int id)
        {
            var entity = await _context.Set<T>().FindAsync(id);
            if (entity == null)
                throw new KeyNotFoundException($"Entity af typen {typeof(T).Name} med {id} kan ikke findes.");
            return entity;
        }

        //Ved AddAsync tilføjes objektet til DbContext, og ved SaveChangesAsync(),
        //så gemmes objektet i databasen.T entity er objektet. Task er en asynkron løbende opgave.
        public async Task<T> AddAsync(T entity)
        {
            await _context.Set<T>().AddAsync(entity);
            await _context.SaveChangesAsync();
            return entity;
        }

        //Ved DeleteAsync fjernes objektet fra DbContext, og ved SaveChangesAsync(),
        //gemmes ændringen, den nye DbContext, i databasen.
        public async Task DeleteAsync(T entity)
        {
            _context.Set<T>().Remove(entity);
            await _context.SaveChangesAsync();
        }

        //Ved UpdateAsync opdateres objektet i DbContext, og ved SaveChangesAsync(),
        //gemmes ændringen, den nye DbContext, i databasen.
        public async Task<T> UpdateAsync(T entity)
        {
            await _context.SaveChangesAsync();
            return entity;
        }
    }
}
```
