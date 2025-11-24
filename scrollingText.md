# Scrolling text

### CSS hos gnist.show
```CSS
.movingtext-container {
  overflow: hidden;
  white-space: nowrap;
  position: relative;
  width: 100%;
  margin: 0 auto;
  text-align: center;
  font: 250px Garamond, serif;
}

.word {
  display: inline-block;
  min-width: 100%; 
  animation: scrollText 10s steps(2) infinite; 
  opacity: 1;
}

@keyframes scrollText {
  0%, 16.666% {
    transform: translateX(0);
  }
  33.333%, 50% {
    transform: translateX(-100%);
  }
  66.666%, 83.333% {
    transform: translateX(-200%);
  }
  100% {
    transform: translateX(-200%);
  }
}
```
