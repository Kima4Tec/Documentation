# Cpp Extra


## Loop, input, output, variabler
```
GNU nano 4.8                                                                                       ifElse.cpp
#include <iostream>

using namespace std;

int main() {

char name[20] = "";
int alder = 0;

cout << "\033]0; Festen\007";

cout << "Indtast dit navn: ";
cin >> name;

cout << "\033[44m";

do {
cout << "Indtast din alder: ";
cin >> alder;
if (alder == 26) {
cout << "Jeg tror du lyver, " << name << endl;
}
} while(alder == 26);

if (alder > 60) {
cout << "Du er for gammel, " << name << endl;
}
else if(alder > 50 && alder < 60) {
cout << "Du er hverken for gammel eller for ung, " << name << endl;
}
else {
cout << "Du er for ung, " << name << endl;
}
```

## Skærminput farve
```
#include <iostream>

int main() {

    std::cout << "\033[44m";   // blå baggrund
    std::cout << "Hej verden" << std::endl;

    std::cout << "\033[0m";    // nulstil farver

    return 0;
}
```

---

# Ændre baggrundsfarve i C++

### Eksempel

```cpp
#include <iostream>

int main() {

    std::cout << "\033[44m";   // blå baggrund
    std::cout << "Hej verden" << std::endl;

    std::cout << "\033[0m";    // nulstil farver

    return 0;
}
```

`"\033"` er **escape-tegnet**.

---

# Baggrundsfarver

| Kode | Farve |
| ---- | ----- |
| 40   | sort  |
| 41   | rød   |
| 42   | grøn  |
| 43   | gul   |
| 44   | blå   |
| 45   | lilla |
| 46   | cyan  |
| 47   | hvid  |

### Eksempel

```cpp
std::cout << "\033[41m"; // rød baggrund
```

---

# Tekstfarver

| Kode | Farve |
| ---- | ----- |
| 30   | sort  |
| 31   | rød   |
| 32   | grøn  |
| 33   | gul   |
| 34   | blå   |
| 35   | lilla |
| 36   | cyan  |
| 37   | hvid  |

### Eksempel

```cpp
std::cout << "\033[31m"; // rød tekst
```
