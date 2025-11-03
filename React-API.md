# React henter data fra SQL database gennem .net api
(Virker også med PostGres)

## React kode:
```
"use client";
import { useEffect, useState, FormEvent, ChangeEvent } from "react";

const API_URL = process.env.NEXT_PUBLIC_API_URL;

interface Project {
  id: number;
  name: string;
  description: string;
}

export default function ProjectsPage() {
  const [projects, setProjects] = useState<Project[]>([]);
  const [form, setForm] = useState({ name: "", description: "" });
  const [loading, setLoading] = useState(false);

  // Fetch projects i en separat async funktion
  async function fetchProjects() {
    const res = await fetch(`${API_URL}/projects`);
    const data: Project[] = await res.json();
    setProjects(data);
  }

  // Asynkront kald i useEffect
  useEffect(() => {
    const loadProjects = async () => {
      await fetchProjects();
    };
    loadProjects();
  }, []); // Dependency array er tomt, så det kører kun ved mount

  async function createProject(e: FormEvent<HTMLFormElement>) {
    e.preventDefault();
    if (!form.name.trim()) return;

    setLoading(true);
    await fetch(`${API_URL}/projects`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(form),
    });
    setForm({ name: "", description: "" });
    setLoading(false);
    fetchProjects();
  }

  function handleChange(
    e: ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ) {
    const { name, value } = e.target;
    setForm((prev) => ({ ...prev, [name]: value }));
  }

  async function deleteProject(id: number) {
    await fetch(`${API_URL}/projects/${id}`, { method: "DELETE" });
    fetchProjects();
  }

  return (
    <main style={{ padding: "2rem" }}>
      <h1>Projects</h1>

      <form onSubmit={createProject} style={{ marginBottom: "2rem" }}>
        <input
          name="name"
          placeholder="Name"
          value={form.name}
          onChange={handleChange}
          required
        />
        <input
          name="description"
          placeholder="Description"
          value={form.description}
          onChange={handleChange}
        />
        <button type="submit" disabled={loading}>
          {loading ? "Saving..." : "Add Project"}
        </button>
      </form>

      <ul>
        {projects.map((p) => (
          <li key={p.id}>
            <b>{p.name}</b> – {p.description}{" "}
            <button onClick={() => deleteProject(p.id)}>🗑️</button>
          </li>
        ))}
      </ul>
    </main>
  );
}

```


## Har desuden oprettet en fil, som jeg har kaldt .env til miljøvariabler og altså URL til apikald
```
NEXT_PUBLIC_API_URL=https://localhost:7061/api

```


## Husk at ændre cors til apps route ofte port 3000
```
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowLocalhost",
        policy =>
        {
            policy.WithOrigins("http://localhost:3000")
                .AllowAnyMethod()
                .AllowAnyHeader()
                .AllowCredentials();
        });
});
```

## Connectionstring til SQL
```
builder.Services.AddDbContext<ApplicationDbContext>(options =>
options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

```

## Connectionstring til Postgres
```
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));

```

**Til Postgres:**
- installér NuGet pakke: npgsql.entityframeworkcore.postgresql
- ændring af appsettings:
```
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=standarddb;Username=postgres;Password=ditEgetPassword"
```
- installer db i fx Docker, opret db:
```
docker exec -it postgres-db psql -U postgres
```
```
CREATE DATABASE standarddb;
```
**til sidst afslut med: \q**
```

opdater migrations og db:
```
dotnet ef migrations add InitialCreate
dotnet ef database update

```


