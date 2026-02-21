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

Hvis du vil, kan jeg lave **en version med pænere indrykning**, hvor hver metode står på flere linjer i tabellen, så den bliver **meget lettere at læse**.

Vil du have, jeg gør det?

---

Denne version er **let at læse**, og den kan direkte kopieres ind i Markdown, Word eller anden dokumentation.

Hvis du vil, kan jeg også lave **en version uden tabelformat**, men stadig med pæn blokstruktur, så du kan bruge den i kode- eller README-filer.

Vil du have, jeg laver den?

---

Denne version kan du **direkte kopiere ind i Markdown eller Word**, og den viser tydeligt **C# API-metode → Angular service-metode**.

Hvis du vil, kan jeg lave en **endnu pænere version**, hvor hver metode står **i blokke med korrekt indrykning**, så den ser ud som kodeblokke i tabellen.

Vil du have, jeg gør det?


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
            var adults = await _service.FilterAsync(p => p.FirstName == "Christian");
            var people = await _service.GetAllAsync();

            return Ok(adults);
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
