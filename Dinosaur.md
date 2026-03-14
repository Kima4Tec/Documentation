# Dinosaur game med JavaScript

```
<!DOCTYPE html>
<html>

<head>
    <title>JS Dino Game</title>
    <style>
        body {
            text-align: center;
            font-family: Courier, monospace;
        }

        canvas {
            background: #fff;
            border-bottom: 2px solid #555;
            display: block;
            margin: 0 auto;
        }
    </style>
</head>

<body>
    <h1>JS Dinosaur Game</h1>
    <i class="c">Tryk på Mellemrum for at hoppe </i>
    <canvas id="gameCanvas" width="600" height="200"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // Spillets variabler
        let score = 0;
        let gameSpeed = 5;
        let isGameOver = false;

        const dinoImage = new Image();
        dinoImage.src = '../images/dino.png';

        // Dino objekt
        const dino = {
            x: 50,
            y: 160,
            width: 40,
            height: 40,
            dy: 0,         // Velocity (lodret hastighed)
            jumpForce: 12,
            gravity: 0.7,
            grounded: false,
            img: dinoImage
        };

        const cactusImage = new Image();
        cactusImage.src = '../images/cactus.png';

        let obstacles = [];
        let spawnTimer = 0;


        function handleInput(e) {
            if (e.code === 'Space' && dino.grounded && !isGameOver) {
                dino.dy = -dino.jumpForce;
                dino.grounded = false;
            }
            if (isGameOver && e.code === 'Space') restart();
        }

        function update() {
            if (isGameOver) return;

            // 1. Dino bevægelse
            dino.dy += dino.gravity;
            dino.y += dino.dy;
            if (dino.y > 160) {
                dino.y = 160;
                dino.dy = 0;
                dino.grounded = true;
            }

            // 2. Håndter kaktusser
            spawnTimer--;
            if (spawnTimer <= 0) {
                // Lav en ny kaktus
                obstacles.push({
                    x: canvas.width,
                    y: 160,
                    width: 20,
                    height: 40
                });
                // Sæt timeren til noget tilfældigt (mellem 60 og 120 frames)
                // Vi gør tallet mindre jo højere gameSpeed er, så de kommer hurtigere!
                spawnTimer = Math.floor(Math.random() * 50) + (100 / (gameSpeed / 5));
            }

            // Løb igennem alle kaktusser
            for (let i = obstacles.length - 1; i >= 0; i--) {
                let o = obstacles[i];
                o.x -= gameSpeed;

                // Collision Detection
                if (dino.x < o.x + o.width &&
                    dino.x + dino.width > o.x &&
                    dino.y < o.y + o.height &&
                    dino.y + dino.height > o.y) {
                    isGameOver = true;
                }

                // Fjern kaktus hvis den er ude af skærmen og giv point
                if (o.x + o.width < 0) {
                    obstacles.splice(i, 1);
                    score++;
                    gameSpeed += 0.1;
                }
            }
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Tegn Dino
            ctx.drawImage(dino.img, dino.x, dino.y, dino.width, dino.height);

            // Tegn alle kaktusser i listen
            obstacles.forEach(o => {
                ctx.drawImage(cactusImage, o.x, o.y, o.width, o.height);
            });

            // Score og Game Over
            ctx.fillStyle = 'black';
            ctx.font = '20px Courier, monospace';
            ctx.fillText(`Score: ${score}`, 20, 30);

            if (isGameOver) {
                ctx.fillText('GAME OVER - Tryk Mellemrum', 150, 100);
            }
        }

        function restart() {
            score = 0;
            gameSpeed = 5;
            isGameOver = false;
            obstacles = []; // Tøm listen!
            spawnTimer = 0;
            gameLoop();
        }

        function gameLoop() {
            update();
            draw();
            if (!isGameOver) requestAnimationFrame(gameLoop);
        }

        window.addEventListener('keydown', handleInput);
        gameLoop();
    </script>
</body>

</html>
```
