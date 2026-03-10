# C++

## Indhold
[Typer](#Typer)  
[Include](#Include)   
[Namespace](#Namespace)  
[Metoder](#Metoder)  
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


## Metoder
### cout

## Pointers

## Loops

## Betingelser


