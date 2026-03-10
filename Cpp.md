# C++

## Indhold
[Typer](#Typer)
[Include](#Include)



## Typer

**1. Primitive typer – int, double, char, bool**  
**2. Derived typer – pointer, arrays, references**  
**3. User-defined typer – struct, class, enum**  

I **C++** findes der flere forskellige **typer (data types)**. De kan groft opdeles i **primitive typer**, **afledte typer** og **brugerdefinerede typer**.

---

# 1. Primitive typer (Built-in types)

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

# 2. Afledte typer (Derived types)

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

# 3. Brugerdefinerede typer (User-defined)

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

# 4. Moderne C++ typer

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
## Include

I **C++** bruger man **`#include`** til at importere funktioner, klasser og biblioteker fra standardbiblioteket eller egne filer.

Her er nogle af de **mest brugte includes**.

---

# 1. Input og output

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

# 2. Strings

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

# 3. Vektorer (dynamiske arrays)

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

# 4. Matematik

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

# 5. Algoritmer

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

# 6. Input/output med filer

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

# 7. Tidsfunktioner

```cpp
#include <ctime>
```

Bruges til:

* `time()`
* `clock()`

---

# 8. Containers (datastrukturer)

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

# 9. Utility funktioner

```cpp
#include <utility>
```

Bruges til:

* `std::pair`

---

# 10. C-style funktioner

```cpp
#include <cstdlib>
```

Bruges til:

* `rand()`
* `exit()`

---

# Typisk “standard start” i mange programmer

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




## Namespaces
####  std

## Metoder
### cout


