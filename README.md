# Rouge-games
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Retro Arcade Hub</title>
    <style>
        :root {
            --bg-color: #121212;
            --card-bg: #1e1e1e;
            --primary: #00ff88; /* Neon Green */
            --secondary: #00ccff; /* Neon Blue */
            --accent: #ff0055; /* Neon Pink */
            --text-main: #ffffff;
            --text-muted: #aaaaaa;
            --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: var(--font-family);
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            flex-direction: column;
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* Header */
        header {
            background: linear-gradient(90deg, #1a1a1a 0%, #252525 100%);
            padding: 1rem 2rem;
            border-bottom: 2px solid var(--primary);
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.5);
        }

        h1 {
            font-size: 1.8rem;
            text-transform: uppercase;
            letter-spacing: 2px;
            color: var(--primary);
            text-shadow: 0 0 10px rgba(0, 255, 136, 0.5);
        }

        .status-bar {
            font-size: 0.9rem;
            color: var(--text-muted);
        }

        /* Main Layout */
        main {
            flex: 1;
            padding: 2rem;
            max-width: 1200px;
            margin: 0 auto;
            width: 100%;
            position: relative;
        }

        /* Game Menu Grid */
        #game-menu {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 2rem;
            animation: fadeIn 0.5s ease-out;
        }

        .game-card {
            background-color: var(--card-bg);
            border: 1px solid #333;
            border-radius: 12px;
            padding: 2rem;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
            position: relative;
            overflow: hidden;
        }

        .game-card:hover {
            transform: translateY(-5px);
            border-color: var(--primary);
            box-shadow: 0 10px 20px rgba(0, 255, 136, 0.1);
        }

        .game-card h2 {
            margin-bottom: 1rem;
            color: var(--secondary);
        }

        .game-card p {
            color: var(--text-muted);
            margin-bottom: 1.5rem;
            line-height: 1.5;
        }

        .play-btn {
            background-color: var(--primary);
            color: #000;
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 50px;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
            transition: transform 0.2s, background-color 0.2s;
        }

        .play-btn:hover {
            background-color: #fff;
            transform: scale(1.05);
        }

        /* Game Container (Hidden by default) */
        #game-container {
            display: none; /* Toggled via JS */
            flex-direction: column;
            align-items: center;
            animation: slideUp 0.4s ease-out;
        }

        .game-header {
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
        }

        .back-btn {
            background: transparent;
            border: 1px solid var(--text-muted);
            color: var(--text-main);
            padding: 0.5rem 1rem;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.2s;
        }

        .back-btn:hover {
            border-color: var(--accent);
            color: var(--accent);
        }

        .score-board {
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--primary);
            font-family: 'Courier New', Courier, monospace;
        }

        /* Canvas */
        canvas {
            background-color: #000;
            border: 2px solid #333;
            border-radius: 8px;
            box-shadow: 0 0 20px rgba(0,0,0,0.8);
            max-width: 100%;
            cursor: crosshair;
        }

        .instructions {
            margin-top: 1rem;
            color: var(--text-muted);
            font-size: 0.9rem;
            text-align: center;
            background: rgba(255,255,255,0.05);
            padding: 0.5rem 1rem;
            border-radius: 4px;
        }

        /* Overlay for Game Over */
        #overlay {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.9);
            padding: 2rem;
            border: 2px solid var(--accent);
            border-radius: 10px;
            text-align: center;
            display: none; /* Hidden by default */
            z-index: 10;
            min-width: 300px;
        }

        #overlay h3 {
            color: var(--accent);
            font-size: 2rem;
            margin-bottom: 1rem;
        }

        #overlay p {
            margin-bottom: 1.5rem;
            font-size: 1.2rem;
        }

        #overlay button {
            background-color: var(--accent);
            color: white;
            border: none;
            padding: 0.8rem 2rem;
            font-size: 1.1rem;
            border-radius: 5px;
            cursor: pointer;
        }

        #overlay button:hover {
            opacity: 0.9;
        }

        /* Footer */
        footer {
            text-align: center;
            padding: 2rem;
            color: #555;
            font-size: 0.8rem;
            margin-top: auto;
        }

        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes slideUp {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .hidden {
            display: none !important;
        }

        /* Mobile Controls Overlay (Visible only on touch devices if needed, simplistic approach) */
        .mobile-controls {
            display: none;
            margin-top: 10px;
            gap: 10px;
        }
        @media (max-width: 768px) {
            .mobile-controls {
                display: flex;
            }
            .d-pad-btn {
                width: 60px;
                height: 60px;
                background: #333;
                border-radius: 50%;
                border: none;
                color: white;
                font-size: 24px;
                touch-action: manipulation;
            }
            .d-pad-btn:active { background: var(--primary); color: black; }
        }
    </style>
</head>
<body>

    <header>
        <h1>Arcade<span style="color:white">Hub</span></h1>
        <div class="status-bar">Select a game to play</div>
    </header>

    <main>
        <!-- Main Menu -->
        <section id="game-menu">
            <!-- Game 1: Snake -->
            <article class="game-card" onclick="gameManager.loadGame('snake')">
                <h2>Neon Snake</h2>
                <p>The classic snake game. Eat the glowing food to grow. Don't hit the walls or yourself!</p>
                <button class="play-btn">Play Now</button>
            </article>

            <!-- Game 2: Pong -->
            <article class="game-card" onclick="gameManager.loadGame('pong')">
                <h2>Cyber Pong</h2>
                <p>Player vs Computer. Use your paddle to deflect the ball past the AI opponent.</p>
                <button class="play-btn">Play Now</button>
            </article>

            <!-- Game 3: Reflex -->
            <article class="game-card" onclick="gameManager.loadGame('reflex')">
                <h2>Reflex Rush</h2>
                <p>Test your reaction speed. Click the targets before they disappear. How fast are you?</p>
                <button class="play-btn">Play Now</button>
            </article>
        </section>

        <!-- Game Area -->
        <section id="game-container">
            <div class="game-header">
                <button class="back-btn" onclick="gameManager.returnToMenu()">← Back to Menu</button>
                <div class="score-board">Score: <span id="score-display">0</span></div>
            </div>

            <div style="position: relative;">
                <canvas id="gameCanvas" width="800" height="500"></canvas>
                
                <!-- Game Over Overlay -->
                <div id="overlay">
                    <h3 id="overlay-title">Game Over</h3>
                    <p>Final Score: <span id="overlay-score">0</span></p>
                    <button onclick="gameManager.restartCurrentGame()">Play Again</button>
                </div>
            </div>

            <div class="instructions" id="game-instructions">
                Instructions will appear here...
            </div>

            <!-- Simple Mobile Controls for Snake/Pong -->
            <div class="mobile-controls" id="mobile-controls">
                <button class="d-pad-btn" id="btn-left">←</button>
                <button class="d-pad-btn" id="btn-up">↑</button>
                <button class="d-pad-btn" id="btn-down">↓</button>
                <button class="d-pad-btn" id="btn-right">→</button>
            </div>
        </section>
    </main>

    <footer>
        &copy; 2023 ArcadeHub. All games run natively in the browser.
    </footer>

    <script>
        /**
         * Global Game Manager
         * Handles switching between menu and game view, and managing the game loop.
         */
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const scoreEl = document.getElementById('score-display');
        const instructionsEl = document.getElementById('game-instructions');
        const overlay = document.getElementById('overlay');
        const overlayTitle = document.getElementById('overlay-title');
        const overlayScore = document.getElementById('overlay-score');

        let animationFrameId;
        let activeGame = null;
        let isGameRunning = false;
        let score = 0;

        // Input State
        const keys = {
            ArrowUp: false,
            ArrowDown: false,
            ArrowLeft: false,
            ArrowRight: false
        };

        // Game Definitions
        const games = {
            snake: {
                name: "Neon Snake",
                instructions: "Use Arrow Keys (or on-screen buttons) to move. Eat green food to grow.",
                color: '#00ff88',
                init: snakeInit,
                update: snakeUpdate,
                draw: snakeDraw,
                onClick: null
            },
            pong: {
                name: "Cyber Pong",
                instructions: "Use Up/Down Arrow Keys or Mouse to move your paddle (Left). First to 10 wins.",
                color: '#00ccff',
                init: pongInit,
                update: pongUpdate,
                draw: pongDraw,
                onClick: null
            },
            reflex: {
                name: "Reflex Rush",
                instructions: "Click the circles with your mouse/tap before they shrink to nothing!",
                color: '#ff0055',
                init: reflexInit,
                update: reflexUpdate,
                draw: reflexDraw,
                onClick: reflexClick
            }
        };

        const gameManager = {
            currentGameId: null,

            loadGame: function(gameId) {
                this.currentGameId = gameId;
                const game = games[gameId];
                
                // UI Updates
                document.getElementById('game-menu').classList.add('hidden');
                document.getElementById('game-container').style.display = 'flex';
                instructionsEl.textContent = game.instructions;
                document.getElementById('mobile-controls').style.display = (gameId === 'reflex') ? 'none' : 'flex';
                
                // Reset State
                score = 0;
                scoreEl.textContent = score;
                overlay.style.display = 'none';
                
                // Initialize Game
                if (game.init) game.init();
                
                isGameRunning = true;
                this.loop();
            },

            returnToMenu: function() {
                isGameRunning = false;
                cancelAnimationFrame(animationFrameId);
                document.getElementById('game-container').style.display = 'none';
                document.getElementById('game-menu').classList.remove('hidden');
            },

            restartCurrentGame: function() {
                this.loadGame(this.currentGameId);
            },

            gameOver: function(finalScore) {
                isGameRunning = false;
                cancelAnimationFrame(animationFrameId);
                
                overlayTitle.textContent = "Game Over";
                overlayScore.textContent = finalScore;
                overlay.style.display = 'block';
            },

            loop: function() {
                if (!isGameRunning) return;

                const game = games[this.currentGameId];
                
                // Clear Canvas
                ctx.fillStyle = '#000';
                ctx.fillRect(0, 0, canvas.width, canvas.height);

                // Update & Draw
                if (game.update) game.update();
                if (game.draw) game.draw();

                animationFrameId = requestAnimationFrame(() => this.loop());
            }
        };

        /**
         * GAME 1: NEON SNAKE
         */
        let snake, food, direction, nextDirection, gridSize, tileCountX, tileCountY;
        let snakeSpeed = 0;
        
        function snakeInit() {
            gridSize = 20;
            tileCountX = canvas.width / gridSize;
            tileCountY = canvas.height / gridSize;
            
            snake = [{x: 10, y: 10}];
            direction = {x: 1, y: 0};
            nextDirection = {x: 1, y: 0};
            placeFood();
            snakeSpeed = 0;
        }

        function placeFood() {
            food = {
                x: Math.floor(Math.random() * tileCountX),
                y: Math.floor(Math.random() * tileCountY)
            };
            // Ensure food doesn't spawn on snake
            for(let part of snake) {
                if(part.x === food.x && part.y === food.y) {
                    placeFood();
                    break;
                }
            }
        }

        function snakeUpdate() {
            // Control speed (limit updates)
            snakeSpeed++;
            if (snakeSpeed < 5) return; // Adjust for difficulty
            snakeSpeed = 0;

            direction = nextDirection;
            const head = {x: snake[0].x + direction.x, y: snake[0].y + direction.y};

            // Collision Walls
            if (head.x < 0 || head.x >= tileCountX || head.y < 0 || head.y >= tileCountY) {
                gameManager.gameOver(score);
                return;
            }

            // Collision Self
            for (let part of snake) {
                if (head.x === part.x && head.y === part.y) {
                    gameManager.gameOver(score);
                    return;
                }
            }

            snake.unshift(head);

            // Eat Food
            if (head.x === food.x && head.y === food.y) {
                score += 10;
                scoreEl.textContent = score;
                placeFood();
            } else {
                snake.pop();
            }
        }

        function snakeDraw() {
            ctx.fillStyle = games.snake.color;
            for (let part of snake) {
                ctx.fillRect(part.x * gridSize, part.y * gridSize, gridSize - 2, gridSize - 2);
            }
            
            ctx.fillStyle = games.reflex.color; // Red food
            ctx.beginPath();
            let foodX = food.x * gridSize + gridSize/2;
            let foodY = food.y * gridSize + gridSize/2;
            ctx.arc(foodX, foodY, gridSize/2 - 2, 0, Math.PI * 2);
            ctx.fill();
        }

        /**
         * GAME 2: CYBER PONG
         */
        let paddleHeight = 80, paddleWidth = 10;
        let playerY, aiY;
        let ballX, ballY, ballSpeedX, ballSpeedY;
        let playerScore = 0, aiScore = 0;

        function pongInit() {
            playerY = canvas.height / 2 - paddleHeight / 2;
            aiY = canvas.height / 2 - paddleHeight / 2;
            resetBall();
            playerScore = 0;
            aiScore = 0;
            updatePongScore();
        }

        function resetBall() {
            ballX = canvas.width / 2;
            ballY = canvas.height / 2;
            ballSpeedX = -5; // Start towards player
            ballSpeedY = 3 * (Math.random() > 0.5 ? 1 : -1);
        }

        function updatePongScore() {
            score = playerScore; // Global score for UI
            scoreEl.textContent = `${playerScore} - ${aiScore}`;
        }

        function pongUpdate() {
            // Move Player
            // Keyboard
            if (keys.ArrowUp && playerY > 0) playerY -= 7;
            if (keys.ArrowDown && playerY < canvas.height - paddleHeight) playerY += 7;
            
            // Mouse Move Logic (add event listener separately)
            // But here we just update based on keys for simplicity in loop

            // Move AI (Simple tracking)
            let aiCenter = aiY + paddleHeight / 2;
            if (aiCenter < ballY - 35) aiY += 5;
            else if (aiCenter > ballY + 35) aiY -= 5;
            // Clamp AI
            aiY = Math.max(0, Math.min(canvas.height - paddleHeight, aiY));

            // Move Ball
            ballX += ballSpeedX;
            ballY += ballSpeedY;

            // Bounce Top/Bottom
            if (ballY <= 0 || ballY >= canvas.height) {
                ballSpeedY = -ballSpeedY;
            }

            // Paddle Collisions
            // Player
            if (ballX <= 20 + paddleWidth && ballY >= playerY && ballY <= playerY + paddleHeight) {
                ballSpeedX = -ballSpeedX;
                // Add some spin/angle based on hit position
                let deltaY = ballY - (playerY + paddleHeight/2);
                ballSpeedY = deltaY * 0.3;
                // Increase speed slightly
                ballSpeedX *= 1.05;
            }
            // AI
            if (ballX >= canvas.width - 20 - paddleWidth && ballY >= aiY && ballY <= aiY + paddleHeight) {
                ballSpeedX = -ballSpeedX;
                let deltaY = ballY - (aiY + paddleHeight/2);
                ballSpeedY = deltaY * 0.3;
            }

            // Scoring
            if (ballX < 0) {
                aiScore++;
                updatePongScore();
                if(aiScore >= 10) gameManager.gameOver(playerScore + " - " + aiScore);
                else resetBall();
            } else if (ballX > canvas.width) {
                playerScore++;
                score = playerScore; // Update global score for display
                scoreEl.textContent = `${playerScore} - ${aiScore}`;
                if(playerScore >= 10) gameManager.gameOver("You Win!");
                else resetBall();
            }
        }

        function pongDraw() {
            ctx.fillStyle = games.pong.color;
            
            // Net
            ctx.setLineDash([10, 15]);
            ctx.beginPath();
            ctx.moveTo(canvas.width / 2, 0);
            ctx.lineTo(canvas.width / 2, canvas.height);
            ctx.strokeStyle = '#333';
            ctx.stroke();
            ctx.setLineDash([]);

            // Paddles
            ctx.fillRect(20, playerY, paddleWidth, paddleHeight);
            ctx.fillRect(canvas.width - 20 - paddleWidth, aiY, paddleWidth, paddleHeight);

            // Ball
            ctx.beginPath();
            ctx.arc(ballX, ballY, 8, 0, Math.PI*2);
            ctx.fill();
        }

        /**
         * GAME 3: REFLEX RUSH
         */
        let targets = [];
        let spawnTimer = 0;
        let gameTimer = 0;
        const gameDuration = 30; // seconds

        function reflexInit() {
            targets = [];
            spawnTimer = 0;
            gameTimer = gameDuration * 60; // approx frames
            score = 0;
            scoreEl.textContent = score + " | Time: " + gameDuration;
        }

        function reflexUpdate() {
            if (gameTimer > 0) {
                gameTimer--;
                if (gameTimer % 60 === 0) {
                    scoreEl.textContent = score + " | Time: " + Math.floor(gameTimer/60);
                }
            } else {
                gameManager.gameOver(score);
                return;
            }

            spawnTimer++;
            if (spawnTimer > 40) { // Spawn rate
                spawnTarget();
                spawnTimer = 0;
            }

            // Shrink targets
            for (let i = targets.length - 1; i >= 0; i--) {
                targets[i].radius -= 0.2;
                if (targets[i].radius <= 0) {
                    targets.splice(i, 1);
                    // Optional: Penalty for missing?
                }
            }
        }

        function spawnTarget() {
            const radius = 30 + Math.random() * 20;
            targets.push({
                x: Math.random() * (canvas.width - 2 * radius) + radius,
                y: Math.random() * (canvas.height - 2 * radius) + radius,
                radius: radius,
                color: `hsl(${Math.random() * 360}, 100%, 50%)`
            });
        }

        function reflexDraw() {
            for (let t of targets) {
                ctx.fillStyle = t.color;
                ctx.beginPath();
                ctx.arc(t.x, t.y, t.radius, 0, Math.PI * 2);
                ctx.fill();
                
                // Inner ring
                ctx.strokeStyle = '#fff';
                ctx.lineWidth = 2;
                ctx.stroke();
            }
        }

        function reflexClick(e) {
            const rect = canvas.getBoundingClientRect();
            // Scaling logic for responsive canvas
            const scaleX = canvas.width / rect.width;
            const scaleY = canvas.height / rect.height;
            
            const mouseX = (e.clientX - rect.left) * scaleX;
            const mouseY = (e.clientY - rect.top) * scaleY;

            for (let i = targets.length - 1; i >= 0; i--) {
                const t = targets[i];
                const dist = Math.sqrt((mouseX - t.x)**2 + (mouseY - t.y)**2);
                
                if (dist < t.radius) {
                    targets.splice(i, 1);
                    score++;
                    scoreEl.textContent = score + " | Time: " + Math.ceil(gameTimer/60);
                    return; // Click one at a time
                }
            }
        }

        /**
         * EVENT LISTENERS
         */
        
        // Keyboard
        window.addEventListener('keydown', (e) => {
            if (keys.hasOwnProperty(e.code)) {
                keys[e.code] = true;
                if(isGameRunning) {
                    if(gameManager.currentGameId === 'snake') {
                        if (e.code === 'ArrowUp' && direction.y === 0) nextDirection = {x: 0, y: -1};
                        if (e.code === 'ArrowDown' && direction.y === 0) nextDirection = {x: 0, y: 1};
                        if (e.code === 'ArrowLeft' && direction.x === 0) nextDirection = {x: -1, y: 0};
                        if (e.code === 'ArrowRight' && direction.x === 0) nextDirection = {x: 1, y: 0};
                    }
                }
            }
        });

        window.addEventListener('keyup', (e) => {
            if (keys.hasOwnProperty(e.code)) keys[e.code] = false;
        });

        // Mouse Move for Pong
        canvas.addEventListener('mousemove', (e) => {
            if (isGameRunning && gameManager.currentGameId === 'pong') {
                const rect = canvas.getBoundingClientRect();
                const scaleY = canvas.height / rect.height;
                const mouseY = (e.clientY - rect.top) * scaleY;
                playerY = mouseY - paddleHeight / 2;
                // Clamp
                playerY = Math.max(0, Math.min(canvas.height - paddleHeight, playerY));
            }
        });

        // Click for Reflex
        canvas.addEventListener('mousedown', (e) => {
            if (isGameRunning && gameManager.currentGameId === 'reflex') {
                reflexClick(e);
            }
        });

        // Mobile Controls
        const btnLeft = document.getElementById('btn-left');
        const btnRight = document.getElementById('btn-right');
        const btnUp = document.getElementById('btn-up');
        const btnDown = document.getElementById('btn-down');

        const handleMobileInput = (dir) => {
            if(!isGameRunning) return;
            if(gameManager.currentGameId === 'snake') {
                if (dir === 'up' && direction.y === 0) nextDirection = {x: 0, y: -1};
                if (dir === 'down' && direction.y === 0) nextDirection = {x: 0, y: 1};
                if (dir === 'left' && direction.x === 0) nextDirection = {x: -1, y: 0};
                if (dir === 'right' && direction.x === 0) nextDirection = {x: 1, y: 0};
            } else if (gameManager.currentGameId === 'pong') {
                if (dir === 'up') playerY -= 30;
                if (dir === 'down') playerY += 30;
            }
        };

        // Bind touch events to buttons to prevent double firing or delays
        btnLeft.addEventListener('touchstart', (e) => { e.preventDefault(); handleMobileInput('left'); });
        btnRight.addEventListener('touchstart', (e) => { e.preventDefault(); handleMobileInput('right'); });
        btnUp.addEventListener('touchstart', (e) => { e.preventDefault(); handleMobileInput('up'); });
        btnDown.addEventListener('touchstart', (e) => { e.preventDefault(); handleMobileInput('down'); });
        
        // Mouse fallbacks for mobile buttons on desktop testing
        btnLeft.addEventListener('mousedown', () => handleMobileInput('left'));
        btnRight.addEventListener('mousedown', () => handleMobileInput('right'));
        btnUp.addEventListener('mousedown', () => handleMobileInput('up'));
        btnDown.addEventListener('mousedown', () => handleMobileInput('down'));

    </script>
</body>
</html>
