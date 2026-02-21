## API ↔ Angular mapping

---

Perfekt! Her er en **ren tekstversion med pæn indrykning**, så hver metode står overskueligt på flere linjer i tabellen:

---

Ah, nu forstår jeg fuldt ud – du vil have **ren tekstversion af tabellen**, uden kommentarer (`//`) og uden HTML-tags eller backticks. Her er det korrekt renset:

---

| API-Controller (C#)                                                                                                                                                                                                                         | APP-Service (Angular)                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| [HttpGet] public async Task<ActionResult<IEnumerable<Person>>> GetPerson() { var people = await _service.GetAllAsync(); return Ok(people); }                                                                                                | getAll(): Observable<Person[]> { return this.http.get<Person[]>(this.apiUrl); }                                             |
| [HttpGet("{id}")] public async Task<ActionResult<Person>> GetPerson(int id) { var person = await _service.GetByIdAsync(id); if (person == null) return NotFound(); return Ok(person); }                                                     | getById(id: number): Observable<Person> { return this.http.get<Person>(`${this.apiUrl}/${id}`); }                           |
| [HttpPost] public async Task<ActionResult<Person>> PostPerson(PersonDto personDto) { var createdPerson = await _service.CreateAsync(personDto); return Ok(createdPerson); }                                                                 | create(person: PersonDto): Observable<Person> { return this.http.post<Person>(this.apiUrl, person); }                       |
| [HttpPut("{id}")] public async Task<ActionResult<Person>> PutPerson(int id, PersonDto personDto) { var updatedPerson = await _service.UpdateAsync(id, personDto); if (updatedPerson == null) return NotFound(); return Ok(updatedPerson); } | update(id: number, person: PersonDto): Observable<Person> { return this.http.put<Person>(`${this.apiUrl}/${id}`, person); } |
| [HttpDelete("{id}")] public async Task<IActionResult> DeletePerson(int id) { var success = await _service.DeleteAsync(id); if (!success) return NotFound(); return NoContent(); }                                                           | delete(id: number): Observable<void> { return this.http.delete<void>(`${this.apiUrl}/${id}`); }                             |

---


Perfekt! Nu kan vi lave en **fuld tabel med fire kolonner**, som viser hele flowet:

**Controller → Service → Repository → Angular Service**. Jeg laver en **ren tekstversion** uden kommentarer eller HTML-tags, så den er klar til dokumentation:

---

Selvfølgelig! Her er den **fire-kolonne tabel med APP-Service som første kolonne**, så rækkefølgen bliver:

**APP-Service (Angular) → API-Controller → API-Service → Repository**

---

| APP-Service (Angular)                                                                                                       | API-Controller (C#)                                                                                                                                                                                                                         | API-Service (C#)                                                                                                                                                                                                                              | Repository (C#)                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| getAll(): Observable<Person[]> { return this.http.get<Person[]>(this.apiUrl); }                                             | [HttpGet] public async Task<ActionResult<IEnumerable<Person>>> GetPerson() { var people = await _service.FilterAsync(p => p.FirstName == "Christian"); return Ok(adults); }                                                                 | public async Task<IEnumerable<PersonDto>> GetAllAsync() { var persons = await _repository.GetAllAsync(); return persons.ToDto(); }                                                                                                            | public async Task<IEnumerable<Person>> GetAllAsync() { return await _context.Persons.ToListAsync(); } |
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
