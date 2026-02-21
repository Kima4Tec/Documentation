Her er de sat op side-by-side, så du tydeligt kan se mappingen mellem din C# API controller og Angular service.

---

## 🔁 API ↔ Angular mapping

| C# ASP.NET Core API (PeopleController)                                                                                                                                                                                                                                                                                          | Angular TypeScript (PeopleService)                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `csharp<br>// GET: api/People<br>[HttpGet]<br>public async Task<ActionResult<IEnumerable<Person>>> GetPerson()<br>{<br>    var adults = await _service.FilterAsync(p => p.FirstName == "Christian");<br>    return Ok(adults);<br>}<br>`                                                                                        | `ts<br>// GET: api/People<br>getAll(): Observable<Person[]> {<br>  return this.http.get<Person[]>(this.apiUrl);<br>}<br>`                                                    |
| `csharp<br>// GET: api/People/5<br>[HttpGet("{id}")]<br>public async Task<ActionResult<Person>> GetPerson(int id)<br>{<br>    var person = await _service.GetByIdAsync(id);<br>    if (person == null)<br>        return NotFound();<br>    return Ok(person);<br>}<br>`                                                        | ``ts<br>// GET: api/People/{id}<br>getById(id: number): Observable<Person> {<br>  return this.http.get<Person>(`${this.apiUrl}/${id}`);<br>}<br>``                           |
| `csharp<br>// POST: api/People<br>[HttpPost]<br>public async Task<ActionResult<Person>> PostPerson(PersonDto personDto)<br>{<br>    var createdPerson = await _service.CreateAsync(personDto);<br>    return Ok(createdPerson);<br>}<br>`                                                                                       | `ts<br>// POST: api/People<br>create(person: PersonDto): Observable<Person> {<br>  return this.http.post<Person>(this.apiUrl, person);<br>}<br>`                             |
| `csharp<br>// PUT: api/People/{id}<br>[HttpPut("{id}")]<br>public async Task<ActionResult<Person>> PutPerson(int id, PersonDto personDto)<br>{<br>    var updatedPerson = await _service.UpdateAsync(id, personDto);<br>    if (updatedPerson == null)<br>        return NotFound();<br>    return Ok(updatedPerson);<br>}<br>` | ``ts<br>// PUT: api/People/{id}<br>update(id: number, person: PersonDto): Observable<Person> {<br>  return this.http.put<Person>(`${this.apiUrl}/${id}`, person);<br>}<br>`` |
| `csharp<br>// DELETE: api/People/{id}<br>[HttpDelete("{id}")]<br>public async Task<IActionResult> DeletePerson(int id)<br>{<br>    var success = await _service.DeleteAsync(id);<br>    if (!success)<br>        return NotFound();<br>    return NoContent();<br>}<br>`                                                        | ``ts<br>// DELETE: api/People/{id}<br>delete(id: number): Observable<void> {<br>  return this.http.delete<void>(`${this.apiUrl}/${id}`);<br>}<br>``                          |

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
