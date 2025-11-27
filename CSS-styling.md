# CSS Styling

## Mus og tastatur tilstande
| Pseudo-class | Hvornår den aktiveres                 | Eksempel i UI       |
| ------------ | ------------------------------------- | ------------------- |
| `:hover`     | Musen er over elementet               | Knappen lyser op    |
| `:focus`     | Elementet har keyboard/mus fokus      | Inputfelt får ramme |
| `:active`    | Elementet klikkes på (mus holdt nede) | Knappen presses ned |

```
.box-hover-shadow:hover {
    box-shadow: 0 8px 20px purple; /* stærkere og større skygge */
    transform: translateX(5px);     /* løfter boksen lidt op */
}

.box-hover-shadow:focus {
    box-shadow: 0 8px 10px green; /* stærkere og større skygge */
    border:10px solid lightblue; /* bredde + stil + farve */
    transform: translateX(5px);     /* løfter boksen lidt op */
}

.box-hover-shadow:active {
    box-shadow: 0 8px 10px yellowgreen; /* stærkere og større skygge */
    transform: translateX(5px);     /* løfter boksen lidt op */
}
```
Hvis det bliver gjort på en **`<div>`** virker det kun, hvis du sætter tabindex="0" på div'en:
```
    <div class="box-hover-shadow" tabindex="0">
```

## Content

| Værdi         | Hvordan bredden bestemmes                     |
| ------------- | --------------------------------------------- |
| `fit-content` | Så bred som indhold + padding                 |
| `max-content` | Så bred som indhold uden linjeskift           |
| `min-content` | Så smal som muligt uden at indhold overlapper |
| `auto`        | Fylder hele tilgængelige plads                |


## Box-styling

```
.box-hover-shadow {
    padding: 20px 40px;
    margin: 50px auto; /* centrering horisontalt */
    width: fit-content; /* elementets bredde = indhold + padding */
    background-color: blue;
    color: white;
    text-align: center;
    border:10px solid darkblue; /* bredde + stil + farve */
    border-radius: 10px;

    /* Purple skygge */
    box-shadow: 0 4px 10px purple; /*offsetX offsetY blur spread color;*/
    
    /* Smidig overgang ved hover */
    transition: box-shadow 0.3s ease, transform 0.3s ease;
}


/* Hover-effekt */
.box-hover-shadow:hover {
    box-shadow: 0 8px 20px purple; /* stærkere og større skygge */
    transform: translateY(-5px);     /* løfter boksen lidt op */
}
```

## Text-styling
```
/* text-styling */
.text-title {
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-weight: normal;
    font-size: 2.5em;
    line-height: 1.2;
    padding-bottom: 5px;
    }

.text-title-bold {
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-weight: bold;
    font-size: 2.5em;
    line-height: 1.2;
    padding-bottom: 5px;
    }

.text-header {
    font-weight: normal;
    font-size: 2.33em;
    line-height: 1.2;
    padding-bottom: 5px;
}

.text-header2 {
    font-weight: normal;
    font-size: 2em;
    line-height: 1.2;
    padding-bottom: 5px;
}

.text-header3 {
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-weight: normal;
    font-size: 1.7em;
    line-height: 1.2;
    padding-bottom: 5px;
}
.text-header3-bold {
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-weight: 800;
    font-size: 1.7em;
    line-height: 1.2;
    padding-bottom: 5px;
    letter-spacing: 0.7px;
}

.text-header4 {
    font-weight: normal;
    font-size: 1.33em;
    line-height: 1.2;
    padding-bottom: 5px;
}

.text-normal {
    font-family: Verdana, Geneva, Sans-Serif;
    font-weight: normal;
    font-size: 1em;
    letter-spacing: 0.7px;
    }

.text-small {
    font-weight: normal;
    font-size: 0.875em;
}

.text-quote {
    text-align: center;
    font-style: italic;
    font-size: 1em;
}

/* text-styling END*/
```
