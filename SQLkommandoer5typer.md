# SQL-kommandoer | DDL, DQL, DML, DCL og TCL

SQL-kommandoer er de grundlæggende byggesten, der bruges til at udføre handlinger i en database. Disse handlinger inkluderer forespørgsler på data, oprettelse af tabeller, indsættelse af data, ændring af strukturer, sletning af data samt håndtering af brugerrettigheder.

## SQL-kommandoer opdeles typisk i fem kategorier:

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/2111e814-8e37-4623-89fe-956c78c0db50" />

---

## SQL-kommandoer

### 1. DDL – Data Definition Language

DDL (Data Definition Language) består af SQL-kommandoer, der bruges til at definere, ændre og slette databasestrukturer såsom tabeller, indekser og skemaer. DDL arbejder med selve strukturen i databasen og bruges til at oprette og modificere databaseobjekter.

| Kommando     | Beskrivelse                                                                                         | Syntaks                                                              |
| ------------ | --------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| **CREATE**   | Opretter en database eller dens objekter (tabel, indeks, funktion, view, stored procedure, trigger) | CREATE TABLE table_name (column1 data_type, column2 data_type, ...); |
| **DROP**     | Sletter objekter fra databasen                                                                      | DROP TABLE table_name;                                               |
| **ALTER**    | Ændrer strukturen på et eksisterende databaseobjekt                                                 | ALTER TABLE table_name ADD COLUMN column_name data_type;             |
| **TRUNCATE** | Fjerner alle rækker fra en tabel og frigiver allokeret lagerplads                                   | TRUNCATE TABLE table_name;                                           |
| **COMMENT**  | Tilføjer kommentarer til datadictionary                                                             | COMMENT ON TABLE table_name IS 'comment_text';                       |
| **RENAME**   | Omdøber et eksisterende databaseobjekt                                                              | RENAME TABLE old_table_name TO new_table_name;                       |

**Eksempel:**

```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    hire_date DATE
);
```

*I dette eksempel oprettes en ny tabel kaldet `employees` med kolonner for medarbejder-ID, fornavn, efternavn og ansættelsesdato.*

---

### 2. DQL – Data Query Language

DQL (Data Query Language) bruges til at hente data fra databasen. Den primære kommando er `SELECT`, som henter rækker baseret på angivne betingelser. Resultatet returneres som et resultatsæt (en midlertidig tabel), der kan vises eller anvendes i applikationer.

| Kommando     | Beskrivelse                                           | Syntaks                                                                                  |
| ------------ | ----------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **SELECT**   | Henter data fra databasen                             | SELECT column1, column2, ... FROM table_name WHERE condition;                            |
| **FROM**     | Angiver hvilke tabeller data hentes fra               | SELECT column1 FROM table_name;                                                          |
| **WHERE**    | Filtrerer rækker før gruppering eller aggregering     | SELECT column1 FROM table_name WHERE condition;                                          |
| **GROUP BY** | Grupperer rækker med samme værdier i angivne kolonner | SELECT column1, AVG_FUNCTION(column2) FROM table_name GROUP BY column1;                  |
| **HAVING**   | Filtrerer grupperede resultater                       | SELECT column1, AVG_FUNCTION(column2) FROM table_name GROUP BY column1 HAVING condition; |
| **DISTINCT** | Fjerner dubletter fra resultatsættet                  | SELECT DISTINCT column1, column2 FROM table_name;                                        |
| **ORDER BY** | Sorterer resultatsættet efter en eller flere kolonner | SELECT column1 FROM table_name ORDER BY column1 [ASC | DESC];                            |
| **LIMIT**    | Begrænser antallet af returnerede rækker              | SELECT * FROM table_name LIMIT number;                                                   |

> **Bemærk:** DQL består teknisk set kun af én kommando: `SELECT`. De øvrige (FROM, WHERE, GROUP BY, HAVING, ORDER BY, DISTINCT, LIMIT) er klausuler i SELECT-sætningen.

**Eksempel:**

```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE department = 'Sales'
ORDER BY hire_date DESC;
```

Denne forespørgsel henter fornavn, efternavn og ansættelsesdato for medarbejdere i "Sales"-afdelingen, sorteret efter ansættelsesdato i faldende rækkefølge.

---

### 3. DML – Data Manipulation Language

DML (Data Manipulation Language) bruges til at arbejde med data i tabeller. Med DML kan du indsætte nye rækker, opdatere eksisterende data, slette data eller hente information.

| Kommando   | Beskrivelse                   | Syntaks                                                                      |
| ---------- | ----------------------------- | ---------------------------------------------------------------------------- |
| **INSERT** | Indsætter data i en tabel     | INSERT INTO table_name (column1, column2, ...) VALUES (value1, value2, ...); |
| **UPDATE** | Opdaterer eksisterende rækker | UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;    |
| **DELETE** | Sletter rækker fra en tabel   | DELETE FROM table_name WHERE condition;                                      |

**Eksempel:**

```sql
INSERT INTO employees (first_name, last_name, department) 
VALUES ('Jane', 'Smith', 'HR');
```

Denne kommando indsætter en ny række i tabellen `employees` med fornavnet "Jane", efternavnet "Smith" og afdelingen "HR".

---

### 4. DCL – Data Control Language

DCL (Data Control Language) indeholder kommandoer som `GRANT` og `REVOKE`. Disse bruges til at styre rettigheder og adgang til databaseobjekter.

| Kommando   | Beskrivelse                            | Syntaks                                                                                                    |
| ---------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **GRANT**  | Tildeler rettigheder til en bruger     | GRANT privilege_type [(column_list)] ON [object_type] object_name TO user [WITH GRANT OPTION];             |
| **REVOKE** | Fjerner tidligere tildelte rettigheder | REVOKE [GRANT OPTION FOR] privilege_type [(column_list)] ON [object_type] object_name FROM user [CASCADE]; |

**Eksempel:**

```sql
GRANT SELECT, UPDATE ON employees TO user_name;
```

Denne kommando giver brugeren `user_name` rettighed til at læse og opdatere data i tabellen `employees`.

---

### 5. TCL – Transaction Control Language

TCL (Transaction Control Language) styrer transaktioner i databasen. En transaktion er en række handlinger, der behandles som én samlet enhed. En transaktion afsluttes enten med succes (commit) eller annulleres helt (rollback).

| Kommando              | Beskrivelse                             | Syntaks                               |
| --------------------- | --------------------------------------- | ------------------------------------- |
| **BEGIN TRANSACTION** | Starter en ny transaktion               | BEGIN TRANSACTION [transaction_name]; |
| **COMMIT**            | Gemmer alle ændringer i transaktionen   | COMMIT;                               |
| **ROLLBACK**          | Fortryder ændringer i transaktionen     | ROLLBACK;                             |
| **SAVEPOINT**         | Opretter et gemmepunkt i en transaktion | SAVEPOINT savepoint_name;             |

**Eksempel:**

```sql
BEGIN TRANSACTION;

UPDATE employees 
SET department = 'Marketing' 
WHERE department = 'Sales';

SAVEPOINT before_update;

UPDATE employees 
SET department = 'IT' 
WHERE department = 'HR';

ROLLBACK TO SAVEPOINT before_update;

COMMIT;
```

I dette eksempel:

* En transaktion startes
* Medarbejdere i Sales flyttes til Marketing
* Der oprettes et gemmepunkt
* Medarbejdere i HR flyttes til IT
* Den sidste ændring fortrydes tilbage til gemmepunktet
* Transaktionen gennemføres med COMMIT

---

Oversat fra: *https://www.geeksforgeeks.org/sql/sql-ddl-dql-dml-dcl-tcl-commands/*
