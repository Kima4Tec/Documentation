## Grid
```html
  <div className="grid grid-cols-3 grid-rows-2 gap-4 p-10 mt-8 w-full max-w-md">
  <div className="bg-blue-300 flex justify-center items-center">1</div>
  <div className="bg-blue-300 flex justify-center items-center">2</div>
  <div className="bg-blue-300 flex justify-center items-center">3</div>
  <div className="bg-blue-300 flex justify-center items-center">4</div>
  <div className="bg-blue-300 flex justify-center items-center">5</div>
  <div className="bg-blue-300 flex justify-center items-center">6</div>
</div>
```

## Flex
Et element med display: flex bliver en flex-container. Dets børn (flex-items) kan så arrangeres fleksibelt. 
Hovedakse (main axis): Retningen, hvor flex-items arrangeres (standard: vandret, row).   
Tværakse (cross axis): Vinkelret på hovedaksen (standard: lodret).

### Justering af elementer langs hovedaksen
- justify-start → mod start
- justify-center → centreret
- justify-end → mod slut
- justify-between → mellemrum mellem
- justify-around → lige margin omkring

### Justering af elementer langs tværaksen
- items-start → top
- items-center → centreret
- items-end → bund
- items-stretch → strækker sig

flex-wrap → tillader linjeskift

```html
<div className="flex flex-row justify-between w-1/2 mt-5">
<div className="flex flex-row gap-5">
    <div className="text-black border-black border-2 p-3">Første i flex</div>
    <div className="text-black border-black border-2 p-3">Andet i flex</div>
    <div className="text-black border-black border-2 p-3">Tredje i flex</div></div>
    <div className="text-black border-black border-2 p-3">Fjerde i flex</div>
</div>
```

## Buttons

```html
    const [message, setMessage] = React.useState("");

<div className="flex gap-5">
            <button className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded shadow-md"
            onClick={() => setMessage("Button clicked!")}>
                Click here
            </button>
            <button className="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded shadow-md"
            onClick={() => setMessage("")}>
                Reset
            </button>
            </div>
```
