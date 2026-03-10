# C++

## Indhold
[Typer](#Typer)  
[Include](#Include)   
[Namespace](#Namespace)  
[Funktioner](#Funktioner)  
[Pointers](#Pointers)  
[Loops](#Loops)  
[Betingelser](#Betingelser)



# Typer

**1. Primitive typer – int, double, char, bool**  
**2. Derived typer – pointer, arrays, references**  
**3. User-defined typer – struct, class, enum**  

I **C++** findes der flere forskellige **typer (data types)**. De kan groft opdeles i **primitive typer**, **afledte typer** og **brugerdefinerede typer**.

---

## 1. Primitive typer (Built-in types)

### Heltal (integers)

* `int`
* `short`
* `long`
* `long long`

Med fortegn:

* `signed int`
* `unsigned int`
* `unsigned short`
* `unsigned long`
* `unsigned long long`

Eksempel:

```cpp
int a = 10;
unsigned int b = 20;
long c = 100000;
```

---

### Decimaltal (floating point)

* `float`
* `double`
* `long double`

Eksempel:

```cpp
float f = 3.14;
double d = 3.1415926535;
```

---

### Tegn

* `char`
* `signed char`
* `unsigned char`

Eksempel:

```cpp
char letter = 'A';
```

---

### Boolean

* `bool`

Eksempel:

```cpp
bool isRunning = true;
```

---

### Specielle

* `void` (ingen værdi)
* `nullptr_t` (null pointer type)

Eksempel:

```cpp
void myFunction() {
}
```

---

## 2. Afledte typer (Derived types)

### Pointer

```cpp
int* ptr;
```

### Reference

```cpp
int& ref = x;
```

### Array

```cpp
int numbers[5];
```

### Function

```cpp
int add(int a, int b);
```

---

## 3. Brugerdefinerede typer (User-defined)

### Struct

```cpp
struct Person {
    string name;
    int age;
};
```

### Class

```cpp
class Car {
public:
    int speed;
};
```

### Enum

```cpp
enum Color {
    Red,
    Green,
    Blue
};
```

### Typedef / using

```cpp
typedef unsigned int uint;
using uint = unsigned int;
```

---

## 4. Moderne C++ typer

### `auto`

```cpp
auto x = 10;
```

### `decltype`

```cpp
int x = 5;
decltype(x) y = 10;
```

---
# Include

I **C++** bruger man **`#include`** til at importere funktioner, klasser og biblioteker fra standardbiblioteket eller egne filer.

Her er nogle af de **mest brugte includes**.

---

## 1. Input og output

```cpp
#include <iostream>
```

Bruges til **input og output** via:

* `std::cout`
* `std::cin`
* `std::endl`

Eksempel:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello" << std::endl;
}
```

---

## 2. Strings

```cpp
#include <string>
```

Bruges til:

* `std::string`

Eksempel:

```cpp
#include <string>

std::string name = "Peter";
```

---

## 3. Vektorer (dynamiske arrays)

```cpp
#include <vector>
```

Bruges til:

* `std::vector`

Eksempel:

```cpp
#include <vector>

std::vector<int> numbers = {1,2,3};
```

---

## 4. Matematik

```cpp
#include <cmath>
```

Bruges til funktioner som:

* `sqrt()`
* `pow()`
* `sin()`
* `cos()`

Eksempel:

```cpp
#include <cmath>

double x = sqrt(25);
```

---

## 5. Algoritmer

```cpp
#include <algorithm>
```

Bruges til funktioner som:

* `sort()`
* `find()`
* `max()`
* `min()`

Eksempel:

```cpp
#include <algorithm>

sort(v.begin(), v.end());
```

---

## 6. Input/output med filer

```cpp
#include <fstream>
```

Bruges til:

* `ofstream`
* `ifstream`

Eksempel:

```cpp
#include <fstream>

std::ofstream file("data.txt");
```

---

## 7. Tidsfunktioner

```cpp
#include <ctime>
```

Bruges til:

* `time()`
* `clock()`

---

## 8. Containers (datastrukturer)

### Map

```cpp
#include <map>
```

### Set

```cpp
#include <set>
```

### Stack

```cpp
#include <stack>
```

### Queue

```cpp
#include <queue>
```

---

## 9. Utility funktioner

```cpp
#include <utility>
```

Bruges til:

* `std::pair`

---

## 10. C-style funktioner

```cpp
#include <cstdlib>
```

Bruges til:

* `rand()`
* `exit()`

---

## Typisk “standard start” i mange programmer

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
```

---

✅ **De 5 mest brugte i praksis**

```
<iostream>
<string>
<vector>
<algorithm>
<cmath>
```

---




## Namespace
I **C++** bruges **namespaces** til at organisere kode og undgå navnekonflikter. Det mest kendte er **`std`**, men der findes flere i standardbiblioteket og i praksis.

Her er nogle af de **mest brugte namespaces**.

---

# 1. `std` (Standard Library)

Det **klart mest brugte namespace**.

Det indeholder næsten hele standardbiblioteket.

Eksempler:

```cpp
std::cout
std::cin
std::string
std::vector
std::sort
```

Eksempel:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello" << std::endl;
}
```

Man skriver ofte:

```cpp
using namespace std;
```

så man kan skrive:

```cpp
cout << "Hello";
```

---

# 2. `std::chrono`

Bruges til **tid og målinger**.

```cpp
#include <chrono>
```

Eksempel:

```cpp
std::chrono::system_clock
std::chrono::milliseconds
```

---

# 3. `std::filesystem`

Bruges til **arbejde med filer og mapper**.

```cpp
#include <filesystem>
```

Eksempel:

```cpp
std::filesystem::path
std::filesystem::exists()
```

---

# 4. `std::this_thread`

Bruges til **multithreading**.

```cpp
#include <thread>
```

Eksempel:

```cpp
std::this_thread::sleep_for()
```

---

# 5. `std::ranges` (moderne C++)

Bruges til **algoritmer og collections**.

```cpp
#include <ranges>
```

Eksempel:

```cpp
std::ranges::sort()
```

---

# 6. `std::literals`

Bruges til **string literals og tidsenheder**.

```cpp
using namespace std::literals;
```

Eksempel:

```cpp
auto text = "hello"s;   // string literal
```

---

# 7. `std::placeholders`

Bruges til **function binding**.

```cpp
using namespace std::placeholders;
```

Eksempel:

```cpp
std::bind(func, _1, 5);
```

---

# Oversigt

| Namespace           | Brug               |
| ------------------- | ------------------ |
| `std`               | standardbibliotek  |
| `std::chrono`       | tid                |
| `std::filesystem`   | filer              |
| `std::this_thread`  | threads            |
| `std::ranges`       | moderne algoritmer |
| `std::literals`     | literals           |
| `std::placeholders` | funktion binding   |

---

✅ **I 90% af programmer bruger man kun:**

```cpp
using namespace std;
```

---

💡 Et **meget vigtigt eksamenstip**:
Mange lærere foretrækker faktisk at man skriver:

```cpp
std::cout
std::string
std::vector
```

i stedet for:

```cpp
using namespace std;
```
---


## Funktioner
Her er en liste over de **mest brugte funktioner i C++**, som man typisk støder på i undervisning, eksamen og små programmer. Jeg grupperer dem efter kategori for overskuelighed:

---

## 1. Input / Output

| Funktion         | Bibliotek    | Brug                       |
| ---------------- | ------------ | -------------------------- |
| `std::cout`      | `<iostream>` | Output til terminal        |
| `std::cin`       | `<iostream>` | Input fra terminal         |
| `std::cerr`      | `<iostream>` | Output af fejlbeskeder     |
| `std::getline()` | `<string>`   | Læse hele linjer fra input |

**Eksempel:**

```cpp id="exio1"
#include <iostream>
#include <string>
using namespace std;

int main() {
    string name;
    cout << "Enter name: ";
    getline(cin, name);      // læser hele linjen inkl. mellemrum
    cout << "Hello " << name << endl;
}
```

---

## 2. Matematik (cmath)

| Funktion                       | Bibliotek | Brug                       |
| ------------------------------ | --------- | -------------------------- |
| `sqrt()`                       | `<cmath>` | Kvadratrod                 |
| `pow()`                        | `<cmath>` | Potens                     |
| `abs()`                        | `<cmath>` | Absolutværdi               |
| `sin()`, `cos()`, `tan()`      | `<cmath>` | Trigonometriske funktioner |
| `round()`, `ceil()`, `floor()` | `<cmath>` | Afrunding                  |

**Eksempel:**

```cpp id="exmath"
#include <iostream>
#include <cmath>
using namespace std;

int main() {
    double x = 2.0;
    cout << sqrt(x) << endl;   // 1.41421
    cout << pow(x, 3) << endl; // 8
}
```

---

## 3. Strings

| Funktion / operator     | Bibliotek  | Brug                         |
| ----------------------- | ---------- | ---------------------------- |
| `+`                     | `<string>` | Sammenkædning af strenge     |
| `.size()` / `.length()` | `<string>` | Længde af string             |
| `.substr()`             | `<string>` | Uddrag af string             |
| `.find()`               | `<string>` | Find substring               |
| `.c_str()`              | `<string>` | Konverter til C-style string |

**Eksempel:**

```cpp id="exstr"
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text = "Hello, world!";
    cout << text.substr(0,5) << endl; // "Hello"
    cout << text.size() << endl;       // 13
}
```

---

## 4. Vektorer (Containers)

| Funktion / operator | Bibliotek  | Brug                                |
| ------------------- | ---------- | ----------------------------------- |
| `.push_back()`      | `<vector>` | Tilføj element                      |
| `.size()`           | `<vector>` | Antal elementer                     |
| `.at(index)`        | `<vector>` | Adgang til element med bounds check |
| `.begin() / .end()` | `<vector>` | Iteratorer til loop / algoritmer    |

**Eksempel:**

```cpp id="exvec"
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> nums = {1,2,3};
    nums.push_back(4);
    for(int n : nums) cout << n << " ";
}
```

---

## 5. Algoritmer (algorithm)

| Funktion          | Bibliotek     | Brug               |
| ----------------- | ------------- | ------------------ |
| `sort()`          | `<algorithm>` | Sortere container  |
| `find()`          | `<algorithm>` | Find element       |
| `max()` / `min()` | `<algorithm>` | Maksimum / minimum |
| `reverse()`       | `<algorithm>` | Vend rækkefølge    |

**Eksempel:**

```cpp id="exalg"
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> v = {3,1,4,2};
    sort(v.begin(), v.end());
    for(int n : v) cout << n << " "; // 1 2 3 4
}
```

---

## 6. Filer (fstream)

| Funktion / klasse      | Bibliotek   | Brug             |
| ---------------------- | ----------- | ---------------- |
| `ofstream`             | `<fstream>` | Skriv til fil    |
| `ifstream`             | `<fstream>` | Læs fra fil      |
| `.open()` / `.close()` | `<fstream>` | Åbn / luk fil    |
| `.read()` / `.write()` | `<fstream>` | Binærfil-læsning |

**Eksempel:**

```cpp id="exfile"
#include <fstream>
#include <string>
using namespace std;

int main() {
    ofstream out("file.txt");
    out << "Hello file!" << endl;
    out.close();
}
```

---

## 7. Tid (chrono)

| Funktion / klasse                  | Bibliotek  | Brug               |
| ---------------------------------- | ---------- | ------------------ |
| `std::chrono::system_clock::now()` | `<chrono>` | Hent nuværende tid |
| `std::chrono::duration`            | `<chrono>` | Tidsvarighed       |
| `std::this_thread::sleep_for()`    | `<thread>` | Pause program      |

---

💡 **Kort opsummering af de absolut mest brugte funktioner:**

```
cout, cin, getline
push_back, size
sort, find, max, min
sqrt, pow, abs
```

---

# Funktioner Cheatsheet
Perfekt! Her er en **kompakt C++ cheatsheet med ca. 50 mest brugte funktioner og kommandoer**, opdelt efter kategori. Den dækker **input/output, strings, vektorer, algoritmer, matematik, filer og tid**, så du hurtigt kan referere til den til eksamen eller små projekter.

---

# **C++ Cheatsheet – de mest brugte funktioner**

---

## 1. **Input / Output (`<iostream>`)**

| Funktion                 | Beskrivelse                     |
| ------------------------ | ------------------------------- |
| `std::cout`              | Skriv til terminal              |
| `std::cin`               | Læs fra terminal                |
| `std::cerr`              | Fejlbeskeder                    |
| `std::endl`              | Ny linje                        |
| `std::getline(cin, var)` | Læs hele linjer inkl. mellemrum |
| `std::flush`             | Tøm output-buffer               |

**Eksempel:**

```cpp
#include <iostream>
#include <string>
using namespace std;

string name;
cout << "Name: ";
getline(cin, name);
cout << "Hello " << name << endl;
```

---

## 2. **Strings (`<string>`)**

| Funktion / operator       | Beskrivelse                  |
| ------------------------- | ---------------------------- |
| `+`                       | Sammenkædning                |
| `.size()` / `.length()`   | Længde af string             |
| `.substr(pos, len)`       | Uddrag substring             |
| `.find(substr)`           | Find substring               |
| `.c_str()`                | Konverter til C-style string |
| `.empty()`                | Tjekker om tom               |
| `.erase(pos, len)`        | Slet substring               |
| `.insert(pos, str)`       | Indsæt substring             |
| `.replace(pos, len, str)` | Erstat substring             |

**Eksempel:**

```cpp
string s = "Hello";
cout << s.substr(0, 2); // "He"
```

---

## 3. **Vektorer (`<vector>`)**

| Funktion / operator   | Beskrivelse             |
| --------------------- | ----------------------- |
| `.push_back(val)`     | Tilføj element          |
| `.size()`             | Antal elementer         |
| `.at(i)`              | Adgang med bounds check |
| `.begin()` / `.end()` | Iteratorer              |
| `.empty()`            | Tjek om vektor er tom   |
| `.clear()`            | Tøm vektor              |
| `.pop_back()`         | Fjern sidste element    |

**Eksempel:**

```cpp
vector<int> v = {1,2,3};
v.push_back(4);
for(int n : v) cout << n << " ";
```

---

## 4. **Algoritmer (`<algorithm>`)**

| Funktion                         | Beskrivelse                      |
| -------------------------------- | -------------------------------- |
| `sort(begin, end)`               | Sorter container                 |
| `reverse(begin, end)`            | Vend rækkefølge                  |
| `find(begin, end, val)`          | Find element                     |
| `max(a, b)` / `min(a,b)`         | Maks / min                       |
| `count(begin, end, val)`         | Tæl forekomster                  |
| `accumulate(begin, end, start)`  | Summering (requires `<numeric>`) |
| `binary_search(begin, end, val)` | Binært søg (sorteret container)  |

**Eksempel:**

```cpp
vector<int> v = {3,1,4};
sort(v.begin(), v.end()); // 1 3 4
```

---

## 5. **Matematik (`<cmath>`)**

| Funktion                     | Beskrivelse                |
| ---------------------------- | -------------------------- |
| `sqrt(x)`                    | Kvadratrod                 |
| `pow(x, y)`                  | Potens                     |
| `abs(x)`                     | Absolutværdi               |
| `round(x)`                   | Afrund                     |
| `ceil(x)`                    | Opad afrund                |
| `floor(x)`                   | Nedad afrund               |
| `sin(x)`, `cos(x)`, `tan(x)` | Trigonometriske funktioner |
| `log(x)` / `log10(x)`        | Logaritme                  |
| `exp(x)`                     | Eksponentialfunktion       |

---

## 6. **Filer (`<fstream>`)**

| Funktion / klasse      | Beskrivelse         |
| ---------------------- | ------------------- |
| `ofstream`             | Skriv til fil       |
| `ifstream`             | Læs fra fil         |
| `.open(filename)`      | Åbn fil             |
| `.close()`             | Luk fil             |
| `.read()` / `.write()` | Binærfil-læsning    |
| `.is_open()`           | Tjek om fil er åben |
| `.getline(file, str)`  | Læs linje fra fil   |

**Eksempel:**

```cpp
ofstream out("data.txt");
out << "Hello file!" << endl;
out.close();
```

---

## 7. **Tid (`<chrono>` og `<thread>`)**

| Funktion                               | Beskrivelse   |
| -------------------------------------- | ------------- |
| `std::chrono::system_clock::now()`     | Nu-tid        |
| `std::chrono::duration`                | Tidsvarighed  |
| `std::this_thread::sleep_for(seconds)` | Pause program |
| `std::chrono::milliseconds(ms)`        | Millisekunder |
| `std::chrono::seconds(s)`              | Sekunder      |

**Eksempel:**

```cpp
#include <chrono>
#include <thread>
this_thread::sleep_for(chrono::seconds(1));
```

---

## 8. **C-Style / Utility (`<cstdlib>` / `<cmath>`)**

| Funktion                            | Beskrivelse         |
| ----------------------------------- | ------------------- |
| `rand()`                            | Tilfældig integer   |
| `srand(seed)`                       | Sæt seed til rand() |
| `exit(code)`                        | Stop program        |
| `system("cls")` / `system("clear")` | Ryd terminal        |

---

## 9. **Pointer & Reference**

| Funktion / operator | Beskrivelse                   |
| ------------------- | ----------------------------- |
| `*ptr`              | Dereference pointer           |
| `&var`              | Adresse                       |
| `->`                | Adgang til medlem via pointer |
| `&`                 | Reference                     |
| `*`                 | Pointer-deklaration           |

---

## 10. **Type og konvertering**

| Funktion / operator           | Beskrivelse                    |
| ----------------------------- | ------------------------------ |
| `static_cast<type>(val)`      | Typekonvertering               |
| `dynamic_cast<type>(ptr)`     | Kør runtime cast til polymorfi |
| `const_cast<type>(val)`       | Fjern const                    |
| `reinterpret_cast<type>(val)` | Lav lav-niveau cast            |
| `sizeof(type)`                | Størrelse i bytes              |
| `auto`                        | Automatisk type                |
| `decltype(var)`               | Hent variabel-type             |

---

💡 **Hurtig huskeregel:**

De funktioner, du vil bruge i 90% af dine små programmer:

```
cout, cin, getline, push_back, size, sort, find, max, min, sqrt, pow, abs
```

---

Hvis du vil, kan jeg lave **en version af cheatsheet’en i ét samlet C++ kode-eksempel**, hvor man **kan copy/paste alle funktionerne og teste dem på én gang**.

Vil du have, at jeg gør det?


## Pointers

---

# 1. Hvad er en pointer?

En **pointer** er en variabel, der **gemmer adressen** på en anden variabel i hukommelsen.

* Normalt gemmer en variabel en **værdi**.
* En pointer gemmer **hvor værdien ligger i hukommelsen**.

---

### Syntax

```cpp id="ptr1"
type* ptr; // deklaration af pointer
```

Eksempel:

```cpp id="ptr2"
int x = 42;      // normal variabel
int* p = &x;     // p er pointer til x
```

* `&x` → **adressen** af x
* `*p` → **værdien** som p peger på

---

# 2. Hvordan bruges en pointer?

### Eksempel 1: læs værdi via pointer

```cpp id="ptr3"
#include <iostream>
using namespace std;

int main() {
    int x = 42;
    int* p = &x;

    cout << "Adresse af x: " << p << endl;
    cout << "Værdi af x via pointer: " << *p << endl;
}
```

Output (eksempel):

```
Adresse af x: 0x7ffeefbff5ac
Værdi af x via pointer: 42
```

---

### Eksempel 2: ændre variabel via pointer

```cpp id="ptr4"
int x = 10;
int* p = &x;

*p = 99;  // ændrer værdien af x via pointer
cout << x; // 99
```

---

# 3. Pointer typer

1. **Pointer til en variabel**

```cpp id="ptr5"
int* p;       // pointer til int
double* d;    // pointer til double
char* c;      // pointer til char
```

2. **Pointer til pointer** (double pointer)

```cpp id="ptr6"
int x = 10;
int* p = &x;
int** pp = &p; // pointer til p
```

---

# 4. Null pointer

En pointer kan **pege på ingenting**:

```cpp id="ptr7"
int* p = nullptr;

if(p == nullptr)
    cout << "Pointer er tom!";
```

---

# 5. Pointer arithmetic (avanceret)

Når man har en pointer til et array, kan man **flytte pointeren**:

```cpp id="ptr8"
int arr[] = {10, 20, 30};
int* p = arr;

cout << *p << endl;      // 10
cout << *(p + 1) << endl; // 20
cout << *(p + 2) << endl; // 30
```

---

# 6. Pointers og arrays

```cpp id="ptr9"
int arr[3] = {1,2,3};
int* p = arr; // arr er pointer til første element

for(int i=0; i<3; i++)
    cout << *(p + i) << " "; // 1 2 3
```

---

# 7. Pointers og funktioner

Man kan **overføre en variabel til en funktion via pointer**, så funktionen kan ændre den direkte.

```cpp id="ptr10"
void addOne(int* p) {
    *p = *p + 1;
}

int main() {
    int x = 5;
    addOne(&x);
    cout << x; // 6
}
```

---

# 8. Pointers vs References

| Pointer                     | Reference                |
| --------------------------- | ------------------------ |
| Kan være null               | Skal pege på noget       |
| Kan ændre hvad den peger på | Peger altid på det samme |
| Brug `*` for at få værdi    | Brug direkte navn        |
| Brug `&` for adresse        | Brug `&` i deklaration   |

---

# 9. Praktiske tips

1. **Installer altid `nullptr` for tomme pointers.**
2. **Husk at frigive dynamisk hukommelse** (`delete`/`delete[]`) hvis du bruger `new`.
3. Brug pointere i funktioner for at ændre værdier **uden return**.
4. Brug **references** hvis du ikke har brug for null.

---

💡 **Kort opsummering**

* Pointer = variabel der gemmer **adresse**.
* `*` = "dereference", få værdien pointeren peger på.
* `&` = "adresse operator", få adressen af variabel.
* Pointere bruges til **arrays, dynamisk hukommelse, funktioner og komplekse datastrukturer**.

---



## Loops

## Betingelser


