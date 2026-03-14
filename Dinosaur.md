# Dinosaur game med JavaScript

```
<!DOCTYPE html>
<html>
<head>
    <title>JS Dino Game</title>
    <style>
        body { text-align: center; font-family: Arial; }
        canvas { background: #f7f7f7; border-bottom: 2px solid #555; display: block; margin: 0 auto; }
    </style>
</head>
<body>
    <h1>JS Dino - Tryk på Mellemrum</h1>
    <canvas id="gameCanvas" width="600" height="200"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // Spillets variabler
        let score = 0;
        let gameSpeed = 5;
        let isGameOver = false;

        // Dino objekt
        const dino = {
            x: 50,
            y: 160,
            width: 40,
            height: 40,
            dy: 0,         // Velocity (lodret hastighed)
            jumpForce: 12,
            gravity: 0.6,
            grounded: false
        };

        // Kaktus objekt
        const cactus = {
            x: canvas.width,
            y: 160,
            width: 20,
            height: 40
        };

        function handleInput(e) {
            if (e.code === 'Space' && dino.grounded && !isGameOver) {
                dino.dy = -dino.jumpForce;
                dino.grounded = false;
            }
            if (isGameOver && e.code === 'Space') restart();
        }

        function update() {
            if (isGameOver) return;

            // 1. Tyngdekraft & Hop
            dino.dy += dino.gravity;
            dino.y += dino.dy;

            // Stop dinoen ved jorden
            if (dino.y > 160) {
                dino.y = 160;
                dino.dy = 0;
                dino.grounded = true;
            }

            // 2. Flyt kaktus
            cactus.x -= gameSpeed;
            if (cactus.x + cactus.width < 0) {
                cactus.x = canvas.width;
                score++;
                gameSpeed += 0.1; // Gør spillet sværere!
            }

            // 3. Collision Detection (AABB)
            if (dino.x < cactus.x + cactus.width &&
                dino.x + dino.width > cactus.x &&
                dino.y < cactus.y + cactus.height &&
                dino.y + dino.height > cactus.y) {
                isGameOver = true;
            }
        }

        function draw() {
            // Ryd lærredet
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Tegn Dino (en grå firkant)
            ctx.fillStyle = '#555';
            ctx.fillRect(dino.x, dino.y, dino.width, dino.height);

            // Tegn Kaktus (en rød firkant)
            ctx.fillStyle = 'red';
            ctx.fillRect(cactus.x, cactus.y, cactus.width, cactus.height);

            // Tegn Score
            ctx.fillStyle = 'black';
            ctx.font = '20px Arial';
            ctx.fillText(`Score: ${score}`, 20, 30);

            if (isGameOver) {
                ctx.fillText('GAME OVER - Tryk Mellemrum for at genstarte', 100, 100);
            }
        }

        function restart() {
            score = 0;
            gameSpeed = 5;
            isGameOver = false;
            cactus.x = canvas.width;
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
