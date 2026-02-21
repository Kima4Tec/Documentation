## API ↔ APP mapping

---
Perfekt! Her er din **fire-kolonne tabel optimeret til GitHub**, med **APP-Service først** og hver metode **på flere linjer**, så den bliver overskuelig og scrollable, når du bruger en kodeblok i README:

```markdown
```

| APP-Service (Angular)            | API-Controller (C#) | API-Service (C#)                                          | Repository (C#)                                        |
| -------------------------------- | ------------------- | --------------------------------------------------------- | ------------------------------------------------------ |
| getAll(): Observable<Person[]> { | [HttpGet]           | public async Task<IEnumerable<PersonDto>> GetAllAsync() { | public async Task<IEnumerable<Person>> GetAllAsync() { |

```
return this.http.get<Person[]>(this.apiUrl);             | public async Task<ActionResult<IEnumerable<Person>>> GetPerson() { | var persons = await _repository.GetAllAsync(); |     return await _context.Persons.ToListAsync();
```

}                                       |     var people = await _service.FilterAsync(p => p.FirstName == "Christian"); | return persons.ToDto();                                     | }
|     return Ok(adults);                                   | }                                                              |
getById(id: number): Observable<Person> { | [HttpGet("{id}")]                                        | public async Task<PersonDto> GetByIdAsync(int id) {            | public Task<Person> GetByIdAsync(int id) {
return this.http.get<Person>(`${this.apiUrl}/${id}`);    | public async Task<ActionResult<Person>> GetPerson(int id) { | var person = await _repository.GetByIdAsync(id);               |     return _context.Persons.FindAsync(id).AsTask();
}                                       |     if (person == null) return NotFound();               | if (person == null) return null;                              | }
|     return Ok(person);                                   | return person.ToDto();                                         |
create(person: PersonDto): Observable<Person> { | [HttpPost]                                            | public async Task<PersonDto> CreateAsync(PersonDto dto) {      | public Task AddAsync(Person person) {
return this.http.post<Person>(this.apiUrl, person);      | public async Task<ActionResult<Person>> PostPerson(PersonDto personDto) { | var person = dto.ToEntity();                                    |     return _context.Persons.AddAsync(person).AsTask();
}                                       |     var createdPerson = await _service.CreateAsync(personDto); | await _repository.AddAsync(person);                             | }
|     return Ok(createdPerson);                             | await _repository.SaveChangesAsync();                             |
|                                                          | return person.ToDto();                                         |
update(id: number, person: PersonDto): Observable<Person> { | [HttpPut("{id}")]                                     | public async Task<PersonDto> UpdateAsync(int id, PersonDto dto) { | public void Update(Person person) {
return this.http.put<Person>(`${this.apiUrl}/${id}`, person); | public async Task<ActionResult<Person>> PutPerson(int id, PersonDto personDto) { | var person = await _repository.GetByIdAsync(id);                 |     _context.Persons.Update(person);
}                                       |     if (updatedPerson == null) return NotFound();       | if (person == null) return null;                                | }
|     return Ok(updatedPerson);                             | person.UpdateFromDto(dto);                                      |
|                                                          | await _repository.SaveChangesAsync();                            |
|                                                          | return person.ToDto();                                         |
delete(id: number): Observable<void> {  | [HttpDelete("{id}")]                                      | public async Task<bool> DeleteAsync(int id) {                  | public void Delete(Person person) {
return this.http.delete<void>(`${this.apiUrl}/${id}`);    | public async Task<IActionResult> DeletePerson(int id) {       | var person = await _repository.GetByIdAsync(id);                |     _context.Persons.Remove(person);
}                                       |     var success = await _service.DeleteAsync(id);        | if (person == null) return false;                               | }
|     if (!success) return NotFound();                     | _repository.Delete(person);                                     |
|     return NoContent();                                   | await _repository.SaveChangesAsync();                            |
|                                                          | return true;                                                   |

```
```

---

✅ **Fordele ved denne version på GitHub:**

1. Koden **bevarer indrykninger** i hver celle.
2. Længere linjer vises med **horisontal scroll** i README.
3. Du kan nemt sammenligne **Angular → Controller → Service → Repository** på én række.
4. Metoderne er **læselige uden at klemme kolonnerne**.

---

Hvis du vil, kan jeg også lave **en version, hvor hver metode står **i én “blok” per HTTP-metode**, så tabellen bliver endnu mere overskuelig på små skærme.

Vil du have, jeg laver den?

---

| APP-Service (Angular)                                                                                                       | API-Controller (C#)                                                                                                                                                                                                                         | API-Service (C#)                                                                                                                                                                                                                              | Repository (C#)                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| getAll(): Observable<Person[]> { return this.http.get<Person[]>(this.apiUrl); }                                             | [HttpGet] public async Task<ActionResult<IEnumerable<Person>>> GetPerson() { var people = await _service.GetAllAsync(); return Ok(people); }                                                                 | public async Task<IEnumerable<PersonDto>> GetAllAsync() { var persons = await _repository.GetAllAsync(); return persons.ToDto(); }                                                                                                            | public async Task<IEnumerable<Person>> GetAllAsync() { return await _context.Persons.ToListAsync(); } |
| getById(id: number): Observable<Person> { return this.http.get<Person>(`${this.apiUrl}/${id}`); }                           | [HttpGet("{id}")] public async Task<ActionResult<Person>> GetPerson(int id) { var person = await _service.GetByIdAsync(id); if (person == null) return NotFound(); return Ok(person); }                                                     | public async Task<PersonDto> GetByIdAsync(int id) { var person = await _repository.GetByIdAsync(id); if (person == null) return null; return person.ToDto(); }                                                                                | public Task<Person> GetByIdAsync(int id) { return _context.Persons.FindAsync(id).AsTask(); }          |
| create(person: PersonDto): Observable<Person> { return this.http.post<Person>(this.apiUrl, person); }                       | [HttpPost] public async Task<ActionResult<Person>> PostPerson(PersonDto personDto) { var createdPerson = await _service.CreateAsync(personDto); return Ok(createdPerson); }                                                                 | public async Task<PersonDto> CreateAsync(PersonDto dto) { var person = dto.ToEntity(); await _repository.AddAsync(person); await _repository.SaveChangesAsync(); return person.ToDto(); }                                                     | public Task AddAsync(Person person) { return _context.Persons.AddAsync(person).AsTask(); }            |
| update(id: number, person: PersonDto): Observable<Person> { return this.http.put<Person>(`${this.apiUrl}/${id}`, person); } | [HttpPut("{id}")] public async Task<ActionResult<Person>> PutPerson(int id, PersonDto personDto) { var updatedPerson = await _service.UpdateAsync(id, personDto); if (updatedPerson == null) return NotFound(); return Ok(updatedPerson); } | public async Task<PersonDto> UpdateAsync(int id, PersonDto dto) { var person = await _repository.GetByIdAsync(id); if (person == null) return null; person.UpdateFromDto(dto); await _repository.SaveChangesAsync(); return person.ToDto(); } | public void Update(Person person) { _context.Persons.Update(person); }                                |
| delete(id: number): Observable<void> { return this.http.delete<void>(`${this.apiUrl}/${id}`); }                             | [HttpDelete("{id}")] public async Task<IActionResult> DeletePerson(int id) { var success = await _service.DeleteAsync(id); if (!success) return NotFound(); return NoContent(); }                                                           | public async Task<bool> DeleteAsync(int id) { var person = await _repository.GetByIdAsync(id); if (person == null) return false; _repository.Delete(person); await _repository.SaveChangesAsync(); return true; }                             | public void Delete(Person person) { _context.Persons.Remove(person); }                                |

---



## 📌 Kort fortalt

| HTTP       | C# Return Type                      | Angular Return Type    |
| ---------- | ----------------------------------- | ---------------------- |
| GET (list) | `ActionResult<IEnumerable<Person>>` | `Observable<Person[]>` |
| GET (id)   | `ActionResult<Person>`              | `Observable<Person>`   |
| POST       | `ActionResult<Person>`              | `Observable<Person>`   |
| PUT        | `ActionResult<Person>`              | `Observable<Person>`   |
| DELETE     | `IActionResult`                     | `Observable<void>`     |

---



#Controller
```
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using NewModel.Dtos;
using NewModel.Interfaces;
using NewModel.Models;
using NewModel.Services;

namespace NewModel.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class PeopleController : ControllerBase
    {
        private readonly IPersonService _service;

        public PeopleController(IPersonService service)
        {
            _service = service;
        }

        // GET: api/People
        [HttpGet]
        public async Task<ActionResult<IEnumerable<Person>>> GetPerson()
        {
            var people = await _service.GetAllAsync();

            return Ok(people);
        }

        // GET: api/People/5    
        [HttpGet("{id}")]
        public async Task<ActionResult<Person>> GetPerson(int id)
        {
            var person = await _service.GetByIdAsync(id);
            if (person == null)
                return NotFound();
            return Ok(person);
        }
        [HttpPost]
        public async Task<ActionResult<Person>> PostPerson(PersonDto personDto)
        {
            var createdPerson = await _service.CreateAsync(personDto);

            return Ok(createdPerson);
        }

        [HttpPut("{id}")]
        public async Task<ActionResult<Person>> PutPerson(int id, PersonDto personDto)
        {
            var updatedPerson = await _service.UpdateAsync(id, personDto);
            if (updatedPerson == null)
                return NotFound();
            return Ok(updatedPerson);
        }

        [HttpDelete("{id}")]
        public async Task<IActionResult> DeletePerson(int id)
        {
            var success = await _service.DeleteAsync(id);
            if (!success)
                return NotFound();
            return NoContent();
        }
    }
}
```

# Angular typescript
```
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Person, PersonDto } from './person.model';

@Injectable({
  providedIn: 'root'
})
export class PeopleService {

  private apiUrl = 'https://localhost:5001/api/People'; 
  // Ret port hvis din .NET API kører på en anden

  constructor(private http: HttpClient) { }

  // GET: api/People
  getAll(): Observable<Person[]> {
    return this.http.get<Person[]>(this.apiUrl);
  }

  // GET: api/People/{id}
  getById(id: number): Observable<Person> {
    return this.http.get<Person>(`${this.apiUrl}/${id}`);
  }

  // POST: api/People
  create(person: PersonDto): Observable<Person> {
    return this.http.post<Person>(this.apiUrl, person);
  }

  // PUT: api/People/{id}
  update(id: number, person: PersonDto): Observable<Person> {
    return this.http.put<Person>(`${this.apiUrl}/${id}`, person);
  }

  // DELETE: api/People/{id}
  delete(id: number): Observable<void> {
    return this.http.delete<void>(`${this.apiUrl}/${id}`);
  }
}
```
