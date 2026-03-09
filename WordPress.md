# Visning af navigation og side

### Navngiv elementet
```
menu-abonnent
```

### Tilpas CSS (Udseende - Temaer - Stilarter --> tre prikker øverst højre hjørne [Ekstra CSS])

#### Tilføj dette:
```
body:not(.logged-in) .menu-abonnent {
    display: none;
}
.logged-in .menu-abonnent {
    display: block;
}
```
