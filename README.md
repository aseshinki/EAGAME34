<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>เกม Platformer วิจัย</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            overflow: hidden;
            background: linear-gradient(to bottom, #87CEEB, #E0F6FF);
        }

        #startScreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        #startScreen h1 {
            color: white;
            font-size: 48px;
            margin-bottom: 30px;
            text-shadow: 3px 3px 6px rgba(0,0,0,0.3);
        }

        #startScreen input {
            padding: 15px 30px;
            font-size: 20px;
            border: none;
            border-radius: 10px;
            margin-bottom: 20px;
            width: 300px;
            text-align: center;
        }

        #startScreen button {
            padding: 15px 50px;
            font-size: 24px;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
        }

        #startScreen button:hover {
            background: #45a049;
            transform: scale(1.05);
        }

        canvas {
            display: block;
            margin: 0 auto;
            background: #87CEEB;
        }

        #ui {
            position: fixed;
            top: 20px;
            left: 20px;
            color: white;
            font-size: 24px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
            z-index: 100;
        }

        .heart {
            display: inline-block;
            color: #ff0000;
            font-size: 30px;
            margin-right: 5px;
        }

        #quizModal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        #quizBox {
            background: white;
            padding: 40px;
            border-radius: 20px;
            max-width: 600px;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
        }

        #quizBox h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 24px;
        }

        .quiz-option {
            display: block;
            width: 100%;
            padding: 15px;
            margin: 10px 0;
            font-size: 18px;
            background: #f0f0f0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .quiz-option:hover {
            background: #e0e0e0;
            transform: translateX(10px);
        }

        #gameOverModal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        #gameOverBox {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            padding: 50px;
            border-radius: 20px;
            text-align: center;
            color: white;
        }

        #gameOverBox h2 {
            font-size: 48px;
            margin-bottom: 20px;
        }

        #gameOverBox p {
            font-size: 24px;
            margin: 10px 0;
        }

        #gameOverBox button {
            margin-top: 30px;
            padding: 15px 40px;
            font-size: 20px;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
        }

        #gameOverBox button:hover {
            background: #45a049;
        }
    </style>
</head>
<body>
    <div id="startScreen">
        <h1>🎮 เกม Platformer วิจัย 🎮</h1>
        <input type="text" id="playerName" placeholder="กรุณาใส่ชื่อของคุณ" maxlength="20">
        <button onclick="startGame()">เริ่มเกม</button>
    </div>

    <div id="ui">
        <div id="hearts"></div>
        <div id="score">คะแนน: 0</div>
        <div id="playerNameDisplay"></div>
    </div>

    <canvas id="gameCanvas"></canvas>

    <div id="quizModal">
        <div id="quizBox">
            <h2 id="quizQuestion"></h2>
            <button class="quiz-option" onclick="answerQuiz(0)"></button>
            <button class="quiz-option" onclick="answerQuiz(1)"></button>
            <button class="quiz-option" onclick="answerQuiz(2)"></button>
            <button class="quiz-option" onclick="answerQuiz(3)"></button>
        </div>
    </div>

    <div id="gameOverModal">
        <div id="gameOverBox">
            <h2 id="endTitle"></h2>
            <p id="finalPlayerName"></p>
            <p id="finalHearts"></p>
            <p id="finalScore"></p>
            <button onclick="location.reload()">เริ่มเกมใหม่</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        canvas.width = 800;
        canvas.height = 600;

        let gameStarted = false;
        let playerName = '';

        // Game state
        const game = {
            hearts: 3,
            score: 0,
            camera: 0,
            isPaused: false,
            invincible: false
        };

        // Player
        const player = {
            x: 100,
            y: 400,
            width: 40,
            height: 50,
            velocityY: 0,
            velocityX: 0,
            speed: 5,
            jumpPower: 15,
            gravity: 0.8,
            onGround: false
        };

        // Input
        const keys = {};
        window.addEventListener('keydown', (e) => { keys[e.key] = true; });
        window.addEventListener('keyup', (e) => { keys[e.key] = false; });

        // Quiz questions about research
        const quizQuestions = [
            {
                question: "ขั้นตอนแรกของการทำวิจัย คือ?",
                options: ["เก็บรวบรวมข้อมูล", "กำหนดปัญหาวิจัย", "วิเคราะห์ข้อมูล", "สรุปผลการวิจัย"],
                correct: 1
            },
            {
                question: "การวิจัยเชิงปริมาณ ใช้ข้อมูลประเภทใด?",
                options: ["ข้อมูลเชิงลึก", "ตัวเลขและสถิติ", "ความรู้สึก", "ความคิดเห็น"],
                correct: 1
            },
            {
                question: "การสุ่มตัวอย่างแบบง่าย คือ?",
                options: ["เลือกตามความสะดวก", "ทุกคนมีโอกาสเท่ากัน", "เลือกแบบเจาะจง", "เลือกตามกลุ่ม"],
                correct: 1
            },
            {
                question: "จรรยาบรรณการวิจัย คือ?",
                options: ["กฎหมายวิจัย", "ข้อบังคับผู้วิจัย", "หลักการประพฤติตนของนักวิจัย", "วิธีการทำวิจัย"],
                correct: 2
            },
            {
                question: "การอ้างอิงในงานวิจัย มีความสำคัญเพราะ?",
                options: ["ทำให้ดูดี", "เพิ่มจำนวนหน้า", "ให้เกียรติเจ้าของผลงาน", "เพิ่มคะแนน"],
                correct: 2
            }
        ];

        let currentQuiz = null;
        let usedQuizzes = [];

        // Platforms
        const platforms = [
            { x: 0, y: 500, width: 400, height: 20 },
            { x: 500, y: 500, width: 300, height: 20 },
            { x: 900, y: 450, width: 200, height: 20 },
            { x: 1200, y: 400, width: 300, height: 20 },
            { x: 1600, y: 350, width: 200, height: 20 },
            { x: 1900, y: 400, width: 400, height: 20 },
            { x: 2400, y: 450, width: 300, height: 20 },
            { x: 2800, y: 500, width: 500, height: 20 },
            { x: 0, y: 580, width: 4000, height: 20 } // Ground
        ];

        // Quiz boxes
        const quizBoxes = [
            { x: 600, y: 420, width: 50, height: 50, used: false },
            { x: 1300, y: 320, width: 50, height: 50, used: false },
            { x: 2100, y: 320, width: 50, height: 50, used: false },
            { x: 2900, y: 420, width: 50, height: 50, used: false }
        ];

        // Enemies
        const enemies = [
            { x: 500, y: 450, width: 40, height: 40, speed: 2, direction: 1, minX: 500, maxX: 750 },
            { x: 1200, y: 350, width: 40, height: 40, speed: 2, direction: 1, minX: 1200, maxX: 1450 },
            { x: 2400, y: 400, width: 40, height: 40, speed: 2, direction: 1, minX: 2400, maxX: 2650 }
        ];

        // Finish line
        const finishLine = { x: 3200, y: 200, width: 100, height: 400 };

        function startGame() {
            const nameInput = document.getElementById('playerName').value.trim();
            if (nameInput === '') {
                alert('กรุณาใส่ชื่อของคุณ');
                return;
            }
            playerName = nameInput;
            document.getElementById('startScreen').style.display = 'none';
            document.getElementById('playerNameDisplay').textContent = `ผู้เล่น: ${playerName}`;
            gameStarted = true;
            gameLoop();
        }

        function updateUI() {
            let heartsHTML = '';
            for (let i = 0; i < game.hearts; i++) {
                heartsHTML += '<span class="heart">❤️</span>';
            }
            document.getElementById('hearts').innerHTML = heartsHTML;
            document.getElementById('score').textContent = `คะแนน: ${game.score}`;
        }

        function showQuiz(quizBox) {
            if (usedQuizzes.length === quizQuestions.length) {
                usedQuizzes = [];
            }

            let availableQuizzes = quizQuestions.filter((_, index) => !usedQuizzes.includes(index));
            let randomIndex = Math.floor(Math.random() * availableQuizzes.length);
            currentQuiz = quizQuestions.findIndex(q => q === availableQuizzes[randomIndex]);
            usedQuizzes.push(currentQuiz);

            const quiz = quizQuestions[currentQuiz];
            document.getElementById('quizQuestion').textContent = quiz.question;
            const options = document.querySelectorAll('.quiz-option');
            quiz.options.forEach((option, index) => {
                options[index].textContent = option;
            });

            game.isPaused = true;
            document.getElementById('quizModal').style.display = 'flex';
        }

        function answerQuiz(answer) {
            const quiz = quizQuestions[currentQuiz];
            if (answer === quiz.correct) {
                game.score++;
                if (game.hearts < 3) game.hearts++;
            } else {
                game.score--;
            }

            document.getElementById('quizModal').style.display = 'none';
            game.isPaused = false;
            updateUI();
        }

        function checkCollision(rect1, rect2) {
            return rect1.x < rect2.x + rect2.width &&
                   rect1.x + rect1.width > rect2.x &&
                   rect1.y < rect2.y + rect2.height &&
                   rect1.y + rect1.height > rect2.y;
        }

        function gameOver() {
            game.isPaused = true;
            document.getElementById('endTitle').textContent = '💀 GAME OVER 💀';
            document.getElementById('finalPlayerName').textContent = `ผู้เล่น: ${playerName}`;
            document.getElementById('finalHearts').textContent = `หัวใจที่เหลือ: 0 ❤️`;
            document.getElementById('finalScore').textContent = `คะแนนสุดท้าย: ${game.score}`;
            document.getElementById('gameOverModal').style.display = 'flex';
        }

        function gameWin() {
            game.isPaused = true;
            document.getElementById('endTitle').textContent = '🎉 ยินดีด้วย! คุณชนะ! 🎉';
            document.getElementById('finalPlayerName').textContent = `ผู้เล่น: ${playerName}`;
            document.getElementById('finalHearts').textContent = `หัวใจที่เหลือ: ${game.hearts} ❤️`;
            document.getElementById('finalScore').textContent = `คะแนนสุดท้าย: ${game.score}`;
            document.getElementById('gameOverModal').style.display = 'flex';
        }

        function update() {
            if (game.isPaused) return;

            // Player movement
            player.velocityX = 0;
            if (keys['ArrowLeft']) {
                player.velocityX = -player.speed;
            }
            if (keys['ArrowRight']) {
                player.velocityX = player.speed;
            }
            if (keys['ArrowUp'] && player.onGround) {
                player.velocityY = -player.jumpPower;
                player.onGround = false;
            }

            // Apply gravity
            player.velocityY += player.gravity;
            player.x += player.velocityX;
            player.y += player.velocityY;

            // Platform collision
            player.onGround = false;
            platforms.forEach(platform => {
                if (checkCollision(player, platform)) {
                    if (player.velocityY > 0) {
                        player.y = platform.y - player.height;
                        player.velocityY = 0;
                        player.onGround = true;
                    }
                }
            });

            // Keep player in bounds
            if (player.x < 0) player.x = 0;
            if (player.y > canvas.height) {
                game.hearts--;
                updateUI();
                if (game.hearts <= 0) {
                    gameOver();
                } else {
                    player.x = 100;
                    player.y = 400;
                    player.velocityY = 0;
                }
            }

            // Camera follow player
            game.camera = player.x - canvas.width / 3;
            if (game.camera < 0) game.camera = 0;

            // Quiz box collision
            quizBoxes.forEach(box => {
                if (!box.used && checkCollision(player, box)) {
                    box.used = true;
                    showQuiz(box);
                }
            });

            // Enemy collision
            if (!game.invincible) {
                enemies.forEach(enemy => {
                    if (checkCollision(player, enemy)) {
                        game.hearts--;
                        updateUI();
                        game.invincible = true;
                        
                        setTimeout(() => {
                            game.invincible = false;
                        }, 1000);

                        if (game.hearts <= 0) {
                            gameOver();
                        }
                    }
                });
            }

            // Enemy movement
            enemies.forEach(enemy => {
                enemy.x += enemy.speed * enemy.direction;
                if (enemy.x <= enemy.minX || enemy.x >= enemy.maxX) {
                    enemy.direction *= -1;
                }
            });

            // Check finish line
            if (checkCollision(player, finishLine)) {
                gameWin();
            }
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Save context and apply camera
            ctx.save();
            ctx.translate(-game.camera, 0);

            // Draw sky gradient
            const gradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
            gradient.addColorStop(0, '#87CEEB');
            gradient.addColorStop(1, '#E0F6FF');
            ctx.fillStyle = gradient;
            ctx.fillRect(game.camera, 0, canvas.width, canvas.height);

            // Draw platforms
            ctx.fillStyle = '#8B4513';
            ctx.strokeStyle = '#654321';
            ctx.lineWidth = 3;
            platforms.forEach(platform => {
                ctx.fillRect(platform.x, platform.y, platform.width, platform.height);
                ctx.strokeRect(platform.x, platform.y, platform.width, platform.height);
                
                // Grass on top
                ctx.fillStyle = '#228B22';
                ctx.fillRect(platform.x, platform.y - 5, platform.width, 5);
                ctx.fillStyle = '#8B4513';
            });

            // Draw quiz boxes
            quizBoxes.forEach(box => {
                if (!box.used) {
                    ctx.fillStyle = '#FFD700';
                    ctx.fillRect(box.x, box.y, box.width, box.height);
                    ctx.strokeStyle = '#FFA500';
                    ctx.lineWidth = 3;
                    ctx.strokeRect(box.x, box.y, box.width, box.height);
                    
                    // Question mark
                    ctx.fillStyle = '#000';
                    ctx.font = 'bold 30px Arial';
                    ctx.textAlign = 'center';
                    ctx.fillText('?', box.x + box.width / 2, box.y + box.height / 2 + 10);
                }
            });

            // Draw enemies
            enemies.forEach(enemy => {
                ctx.fillStyle = '#8B0000';
                ctx.fillRect(enemy.x, enemy.y, enemy.width, enemy.height);
                
                // Eyes
                ctx.fillStyle = '#FFFFFF';
                ctx.fillRect(enemy.x + 8, enemy.y + 10, 8, 8);
                ctx.fillRect(enemy.x + 24, enemy.y + 10, 8, 8);
                ctx.fillStyle = '#000000';
                ctx.fillRect(enemy.x + 10, enemy.y + 12, 4, 4);
                ctx.fillRect(enemy.x + 26, enemy.y + 12, 4, 4);
            });

            // Draw finish line
            ctx.fillStyle = '#FFD700';
            ctx.fillRect(finishLine.x, finishLine.y, finishLine.width, finishLine.height);
            ctx.strokeStyle = '#FFA500';
            ctx.lineWidth = 5;
            ctx.strokeRect(finishLine.x, finishLine.y, finishLine.width, finishLine.height);
            
            // Flag
            ctx.fillStyle = '#FF0000';
            ctx.font = 'bold 40px Arial';
            ctx.textAlign = 'center';
            ctx.fillText('🏁', finishLine.x + finishLine.width / 2, finishLine.y + 50);
            ctx.font = 'bold 24px Arial';
            ctx.fillStyle = '#000';
            ctx.fillText('เส้นชัย', finishLine.x + finishLine.width / 2, finishLine.y + 100);

            // Draw player
            if (!game.invincible || Math.floor(Date.now() / 100) % 2 === 0) {
                ctx.fillStyle = '#0066CC';
                ctx.fillRect(player.x, player.y, player.width, player.height);
                
                // Head
                ctx.fillStyle = '#FFE0BD';
                ctx.fillRect(player.x + 5, player.y, 30, 20);
                
                // Eyes
                ctx.fillStyle = '#000000';
                ctx.fillRect(player.x + 10, player.y + 8, 4, 4);
                ctx.fillRect(player.x + 26, player.y + 8, 4, 4);
                
                // Smile
                ctx.strokeStyle = '#000000';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.arc(player.x + 20, player.y + 12, 6, 0, Math.PI);
                ctx.stroke();
            }

            ctx.restore();
        }

        function gameLoop() {
            if (!gameStarted) return;
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        updateUI();
    </script>
</body>
</html>
