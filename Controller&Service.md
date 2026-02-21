## API ↔ APP mapping

---

| APP-Service (Angular)                                                                                                       | API-Controller (C#)                                                                                                                                                                                                                            | API-Service (C#) (DTO’er uden mapping)                                                                                                                                                                                                                                                                                                                                                                               | Repository (C#)                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| getAll(): Observable<Person[]> { return this.http.get<Person[]>(this.apiUrl); }                                             | [HttpGet] public async Task<ActionResult<IEnumerable<PersonDto>>> GetPerson() { var people = await _service.GetAllAsync(); return Ok(people); }                                                                                                | public async Task<IEnumerable<PersonDto>> GetAllAsync() { var persons = await _repository.GetAllAsync(); return persons.Select(p => new PersonDto { FirstName = p.FirstName, LastName = p.LastName, Age = p.Age }); }                                                                                                                                                                                                | public async Task<IEnumerable<Person>> GetAllAsync() { return await _context.Persons.ToListAsync(); } |
| getById(id: number): Observable<Person> { return this.http.get<Person>(`${this.apiUrl}/${id}`); }                           | [HttpGet("{id}")] public async Task<ActionResult<PersonDto>> GetPerson(int id) { var person = await _service.GetByIdAsync(id); if (person == null) return NotFound(); return Ok(person); }                                                     | public async Task<PersonDto> GetByIdAsync(int id) { var person = await _repository.GetByIdAsync(id); if (person == null) return null; return new PersonDto { FirstName = person.FirstName, LastName = person.LastName, Age = person.Age }; }                                                                                                                                                                         | public Task<Person> GetByIdAsync(int id) { return _context.Persons.FindAsync(id).AsTask(); }          |
| create(person: PersonDto): Observable<Person> { return this.http.post<Person>(this.apiUrl, person); }                       | [HttpPost] public async Task<ActionResult<PersonDto>> PostPerson(PersonDto personDto) { var createdPerson = await _service.CreateAsync(personDto); return Ok(createdPerson); }                                                                 | public async Task<PersonDto> CreateAsync(PersonDto dto) { var person = new Person { FirstName = dto.FirstName, LastName = dto.LastName, Age = dto.Age }; await _repository.AddAsync(person); await _repository.SaveChangesAsync(); return new PersonDto { FirstName = person.FirstName, LastName = person.LastName, Age = person.Age }; }                                                                            | public Task AddAsync(Person person) { return _context.Persons.AddAsync(person).AsTask(); }            |
| update(id: number, person: PersonDto): Observable<Person> { return this.http.put<Person>(`${this.apiUrl}/${id}`, person); } | [HttpPut("{id}")] public async Task<ActionResult<PersonDto>> PutPerson(int id, PersonDto personDto) { var updatedPerson = await _service.UpdateAsync(id, personDto); if (updatedPerson == null) return NotFound(); return Ok(updatedPerson); } | public async Task<PersonDto> UpdateAsync(int id, PersonDto dto) { var person = await _repository.GetByIdAsync(id); if (person == null) return null; person.FirstName = dto.FirstName; person.LastName = dto.LastName; person.Age = dto.Age; _repository.Update(person); await _repository.SaveChangesAsync(); return new PersonDto { FirstName = person.FirstName, LastName = person.LastName, Age = person.Age }; } | public void Update(Person person) { _context.Persons.Update(person); }                                |
| delete(id: number): Observable<void> { return this.http.delete<void>(`${this.apiUrl}/${id}`); }                             | [HttpDelete("{id}")] public async Task<IActionResult> DeletePerson(int id) { var success = await _service.DeleteAsync(id); if (!success) return NotFound(); return NoContent(); }                                                              | public async Task<bool> DeleteAsync(int id) { var person = await _repository.GetByIdAsync(id); if (person == null) return false; _repository.Delete(person); await _repository.SaveChangesAsync(); return true; }                                                                                                                                                                                                    | public void Delete(Person person) { _context.Persons.Remove(person); }                             



---

| API-Service metode (PeopleService)                                                                                          | Component-metode (Angular)                                                                                                                                                                                                                                                                                                                                                                                                                                                       | HTML-template eksempel                                                                                                                                                                                                                                                                |
| --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| getAll(): Observable<Person[]> { return this.http.get<Person[]>(this.apiUrl); }                                             | loadPeople(): void { this.peopleService.getAll().subscribe({ next: (data) => this.people = data, error: (err) => console.error('Fejl ved hentning af personer', err) }); }                                                                                                                                                                                                                                                                                                       | `<button (click)="loadPeople()">Hent alle</button>`<br>`<ul><li *ngFor="let p of people">{{p.firstName}} {{p.lastName}}</li></ul>`                                                                                                                                                    |
| getById(id: number): Observable<Person> { return this.http.get<Person>(`${this.apiUrl}/${id}`); }                           | getPersonById(id: number): void { this.peopleService.getById(id).subscribe({ next: (person) => this.selectedPerson = person, error: (err) => console.error(`Fejl ved hentning af person med id ${id}`, err) }); }                                                                                                                                                                                                                                                                | `<input [(ngModel)]="personId" placeholder="Person ID">`<br>`<button (click)="getPersonById(personId)">Hent person</button>`<br>`<div *ngIf="selectedPerson">{{selectedPerson.firstName}} {{selectedPerson.lastName}}</div>`                                                          |
| create(person: PersonDto): Observable<Person> { return this.http.post<Person>(this.apiUrl, person); }                       | createPerson(): void { this.peopleService.create(this.newPerson).subscribe({ next: (created) => { this.people.push(created); this.newPerson = { firstName: '', lastName: '', age: 0 }; }, error: (err) => console.error('Fejl ved oprettelse af person', err) }); }                                                                                                                                                                                                              | `<input [(ngModel)]="newPerson.firstName" placeholder="Fornavn">`<br>`<input [(ngModel)]="newPerson.lastName" placeholder="Efternavn">`<br>`<input type="number" [(ngModel)]="newPerson.age" placeholder="Alder">`<br>`<button (click)="createPerson()">Opret person</button>`        |
| update(id: number, person: PersonDto): Observable<Person> { return this.http.put<Person>(`${this.apiUrl}/${id}`, person); } | updatePerson(id: number): void { if (!this.selectedPerson) return; const dto: PersonDto = { firstName: this.selectedPerson.firstName, lastName: this.selectedPerson.lastName, age: this.selectedPerson.age }; this.peopleService.update(id, dto).subscribe({ next: (updated) => { const index = this.people.findIndex(p => p.id === id); if (index > -1) this.people[index] = updated; }, error: (err) => console.error(`Fejl ved opdatering af person med id ${id}`, err) }); } | `<div *ngIf="selectedPerson">`<br>`<input [(ngModel)]="selectedPerson.firstName">`<br>`<input [(ngModel)]="selectedPerson.lastName">`<br>`<input type="number" [(ngModel)]="selectedPerson.age">`<br>`<button (click)="updatePerson(selectedPerson.id)">Opdater</button>`<br>`</div>` |
| delete(id: number): Observable<void> { return this.http.delete<void>(`${this.apiUrl}/${id}`); }                             | deletePerson(id: number): void { this.peopleService.delete(id).subscribe({ next: () => { this.people = this.people.filter(p => p.id !== id); }, error: (err) => console.error(`Fejl ved sletning af person med id ${id}`, err) }); }                                                                                                                                                                                                                                             | `<button *ngFor="let p of people" (click)="deletePerson(p.id)">Slet {{p.firstName}}</button>`                                                                                                                                                                                         |

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
