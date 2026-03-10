# C++

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

## Namespaces
####  std

## Metoder
### cout
