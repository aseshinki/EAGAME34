<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lab Runner: Theory of Motion</title>
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
            animation: skyAnimation 10s ease infinite;
        }

        @keyframes skyAnimation {
            0%, 100% { background: linear-gradient(to bottom, #87CEEB, #E0F6FF); }
            50% { background: linear-gradient(to bottom, #FFB6C1, #FFF5E6); }
        }

        #startScreen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            animation: gradientShift 3s ease infinite;
        }

        @keyframes gradientShift {
            0%, 100% { background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%); }
            50% { background: linear-gradient(135deg, #f093fb 0%, #667eea 50%, #764ba2 100%); }
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
            background: linear-gradient(135deg, #FF6B6B, #FFA500, #FFD700, #4CAF50, #4169E1, #9370DB);
            background-size: 300% 300%;
            animation: rainbowButton 3s ease infinite;
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        @keyframes rainbowButton {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        #startScreen button:hover {
            transform: scale(1.1);
            box-shadow: 0 10px 30px rgba(255, 105, 180, 0.5);
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
            text-shadow: 3px 3px 6px rgba(0,0,0,0.8);
            z-index: 100;
            background: linear-gradient(135deg, rgba(255, 105, 180, 0.7), rgba(138, 43, 226, 0.7));
            padding: 15px 25px;
            border-radius: 15px;
            border: 3px solid rgba(255, 255, 255, 0.5);
            font-weight: bold;
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
            background: linear-gradient(135deg, #FFE5E5, #FFF5E5);
            border: 3px solid #FF6B6B;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
        }

        .quiz-option:hover {
            background: linear-gradient(135deg, #FFB6C1, #FFD700);
            transform: translateX(10px) scale(1.05);
            box-shadow: 0 5px 15px rgba(255, 107, 107, 0.5);
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
            background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
            padding: 50px;
            border-radius: 20px;
            text-align: center;
            color: white;
            box-shadow: 0 20px 60px rgba(246, 79, 89, 0.6);
            animation: pulse 2s ease infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.02); }
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
            background: linear-gradient(135deg, #FF6B6B, #FFA500, #FFD700);
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
            transition: all 0.3s;
        }

        #gameOverBox button:hover {
            transform: scale(1.1);
            box-shadow: 0 10px 30px rgba(255, 215, 0, 0.6);
        }
    </style>
</head>
<body>
    <div id="startScreen">
        <h1>🔬 Lab Runner: Theory of Motion 🚀</h1>
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
            document.getElementById('playerNameDisplay').textContent = `🎮 ผู้เล่น: ${playerName}`;
            gameStarted = true;
            gameLoop();
        }

        function updateUI() {
            let heartsHTML = '';
            for (let i = 0; i < game.hearts; i++) {
                heartsHTML += '<span class="heart">❤️</span>';
            }
            document.getElementById('hearts').innerHTML = heartsHTML;
            document.getElementById('score').textContent = `⭐ คะแนน: ${game.score}`;
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
            document.getElementById('finalPlayerName').textContent = `🎮 ผู้เล่น: ${playerName}`;
            document.getElementById('finalHearts').textContent = `💖 หัวใจที่เหลือ: 0 ❤️`;
            document.getElementById('finalScore').textContent = `⭐ คะแนนสุดท้าย: ${game.score} ⭐`;
            document.getElementById('gameOverModal').style.display = 'flex';
        }

        function gameWin() {
            game.isPaused = true;
            document.getElementById('endTitle').textContent = '🎉 ยินดีด้วย! คุณชนะ! 🎉';
            document.getElementById('finalPlayerName').textContent = `🎮 ผู้เล่น: ${playerName}`;
            document.getElementById('finalHearts').textContent = `💖 หัวใจที่เหลือ: ${game.hearts} ❤️`;
            document.getElementById('finalScore').textContent = `⭐ คะแนนสุดท้าย: ${game.score} ⭐`;
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
            const gradient = ctx.createLinearGradient(game.camera, 0, game.camera, canvas.height);
            gradient.addColorStop(0, '#87CEEB');
            gradient.addColorStop(0.5, '#B0E0E6');
            gradient.addColorStop(1, '#E0F6FF');
            ctx.fillStyle = gradient;
            ctx.fillRect(game.camera, 0, canvas.width, canvas.height);
            
            // Draw sun ☀️
            ctx.font = '60px Arial';
            ctx.fillText('☀️', game.camera + 650, 80);
            
            // Draw clouds ☁️
            ctx.font = '40px Arial';
            const cloudPositions = [
                { x: 100, y: 50 },
                { x: 400, y: 100 },
                { x: 800, y: 60 },
                { x: 1200, y: 90 },
                { x: 1600, y: 70 },
                { x: 2000, y: 100 },
                { x: 2400, y: 50 },
                { x: 2800, y: 80 }
            ];
            cloudPositions.forEach(cloud => {
                ctx.fillText('☁️', cloud.x, cloud.y);
                ctx.fillText('☁️', cloud.x + 30, cloud.y + 10);
            });

            // Draw platforms 🏗️
            platforms.forEach((platform, index) => {
                // Create colorful gradient for each platform
                const platformGradient = ctx.createLinearGradient(platform.x, platform.y, platform.x + platform.width, platform.y);
                const colors = [
                    ['#8B4513', '#A0522D'],
                    ['#CD853F', '#DEB887'],
                    ['#D2691E', '#F4A460']
                ];
                const colorPair = colors[index % colors.length];
                platformGradient.addColorStop(0, colorPair[0]);
                platformGradient.addColorStop(1, colorPair[1]);
                
                ctx.fillStyle = platformGradient;
                ctx.fillRect(platform.x, platform.y, platform.width, platform.height);
                ctx.strokeStyle = '#654321';
                ctx.lineWidth = 3;
                ctx.strokeRect(platform.x, platform.y, platform.width, platform.height);
                
                // Grass on top 🌱
                const grassGradient = ctx.createLinearGradient(platform.x, platform.y - 5, platform.x, platform.y);
                grassGradient.addColorStop(0, '#32CD32');
                grassGradient.addColorStop(1, '#228B22');
                ctx.fillStyle = grassGradient;
                ctx.fillRect(platform.x, platform.y - 5, platform.width, 5);
                
                // Add flowers emoji on some platforms 🌸
                if (index % 2 === 0 && platform.width > 100) {
                    ctx.font = '20px Arial';
                    for (let i = 0; i < platform.width; i += 80) {
                        const emojis = ['🌸', '🌼', '🌺', '🌻'];
                        ctx.fillText(emojis[Math.floor(i / 80) % emojis.length], platform.x + i + 20, platform.y - 10);
                    }
                }
            });

            // Draw quiz boxes 📦✨
            quizBoxes.forEach((box, index) => {
                if (!box.used) {
                    // Pulsing glow effect
                    const pulse = Math.sin(Date.now() / 200) * 0.3 + 0.7;
                    
                    // Gradient fill
                    const boxGradient = ctx.createLinearGradient(box.x, box.y, box.x + box.width, box.y + box.height);
                    boxGradient.addColorStop(0, '#FFD700');
                    boxGradient.addColorStop(0.5, '#FFA500');
                    boxGradient.addColorStop(1, '#FF69B4');
                    ctx.fillStyle = boxGradient;
                    ctx.fillRect(box.x, box.y, box.width, box.height);
                    
                    // Glowing border
                    ctx.strokeStyle = `rgba(255, 215, 0, ${pulse})`;
                    ctx.lineWidth = 5;
                    ctx.strokeRect(box.x - 2, box.y - 2, box.width + 4, box.height + 4);
                    
                    // Sparkle effect ✨
                    ctx.font = '16px Arial';
                    ctx.fillText('✨', box.x - 10, box.y + 10);
                    ctx.fillText('✨', box.x + box.width, box.y + 10);
                    ctx.fillText('✨', box.x - 10, box.y + box.height);
                    ctx.fillText('✨', box.x + box.width, box.y + box.height);
                    
                    // Question mark
                    ctx.fillStyle = '#FFF';
                    ctx.font = 'bold 35px Arial';
                    ctx.textAlign = 'center';
                    ctx.strokeStyle = '#000';
                    ctx.lineWidth = 2;
                    ctx.strokeText('?', box.x + box.width / 2, box.y + box.height / 2 + 12);
                    ctx.fillText('?', box.x + box.width / 2, box.y + box.height / 2 + 12);
                }
            });

            // Draw enemies 👾
            enemies.forEach(enemy => {
                // Body gradient
                const enemyGradient = ctx.createLinearGradient(enemy.x, enemy.y, enemy.x + enemy.width, enemy.y + enemy.height);
                enemyGradient.addColorStop(0, '#8B0000');
                enemyGradient.addColorStop(0.5, '#FF0000');
                enemyGradient.addColorStop(1, '#8B0000');
                ctx.fillStyle = enemyGradient;
                ctx.fillRect(enemy.x, enemy.y, enemy.width, enemy.height);
                
                // Outline
                ctx.strokeStyle = '#4B0000';
                ctx.lineWidth = 2;
                ctx.strokeRect(enemy.x, enemy.y, enemy.width, enemy.height);
                
                // Eyes
                ctx.fillStyle = '#FFFF00';
                ctx.beginPath();
                ctx.arc(enemy.x + 12, enemy.y + 15, 6, 0, Math.PI * 2);
                ctx.fill();
                ctx.beginPath();
                ctx.arc(enemy.x + 28, enemy.y + 15, 6, 0, Math.PI * 2);
                ctx.fill();
                
                // Pupils
                ctx.fillStyle = '#000000';
                ctx.beginPath();
                ctx.arc(enemy.x + 12, enemy.y + 15, 3, 0, Math.PI * 2);
                ctx.fill();
                ctx.beginPath();
                ctx.arc(enemy.x + 28, enemy.y + 15, 3, 0, Math.PI * 2);
                ctx.fill();
                
                // Angry eyebrows
                ctx.strokeStyle = '#000000';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.moveTo(enemy.x + 6, enemy.y + 8);
                ctx.lineTo(enemy.x + 18, enemy.y + 12);
                ctx.stroke();
                ctx.beginPath();
                ctx.moveTo(enemy.x + 22, enemy.y + 12);
                ctx.lineTo(enemy.x + 34, enemy.y + 8);
                ctx.stroke();
                
                // Teeth
                ctx.fillStyle = '#FFFFFF';
                for (let i = 0; i < 4; i++) {
                    ctx.fillRect(enemy.x + 8 + i * 8, enemy.y + 28, 6, 8);
                }
                
                // Emoji on top 👾
                ctx.font = '20px Arial';
                ctx.fillText('👾', enemy.x + enemy.width / 2 - 10, enemy.y - 5);
            });

            // Draw finish line 🏁🎉
            const finishGradient = ctx.createLinearGradient(finishLine.x, finishLine.y, finishLine.x, finishLine.y + finishLine.height);
            finishGradient.addColorStop(0, '#FFD700');
            finishGradient.addColorStop(0.2, '#FFA500');
            finishGradient.addColorStop(0.4, '#FF69B4');
            finishGradient.addColorStop(0.6, '#9370DB');
            finishGradient.addColorStop(0.8, '#4169E1');
            finishGradient.addColorStop(1, '#32CD32');
            ctx.fillStyle = finishGradient;
            ctx.fillRect(finishLine.x, finishLine.y, finishLine.width, finishLine.height);
            
            // Glowing border
            const glowPulse = Math.sin(Date.now() / 200) * 0.5 + 0.5;
            ctx.strokeStyle = `rgba(255, 215, 0, ${glowPulse})`;
            ctx.lineWidth = 8;
            ctx.strokeRect(finishLine.x, finishLine.y, finishLine.width, finishLine.height);
            
            // Confetti emojis 🎉
            const confettiEmojis = ['🎉', '🎊', '✨', '⭐', '🌟'];
            ctx.font = '30px Arial';
            for (let i = 0; i < 5; i++) {
                const yPos = finishLine.y + 60 + i * 60;
                ctx.fillText(confettiEmojis[i % confettiEmojis.length], finishLine.x + 10, yPos);
                ctx.fillText(confettiEmojis[(i + 1) % confettiEmojis.length], finishLine.x + finishLine.width - 30, yPos);
            }
            
            // Flag
            ctx.font = 'bold 50px Arial';
            ctx.textAlign = 'center';
            ctx.fillText('🏁', finishLine.x + finishLine.width / 2, finishLine.y + 60);
            
            // Trophy
            ctx.font = 'bold 40px Arial';
            ctx.fillText('🏆', finishLine.x + finishLine.width / 2, finishLine.y + 120);
            
            // Text
            ctx.font = 'bold 28px Arial';
            ctx.fillStyle = '#FFF';
            ctx.strokeStyle = '#000';
            ctx.lineWidth = 3;
            ctx.strokeText('เส้นชัย', finishLine.x + finishLine.width / 2, finishLine.y + 170);
            ctx.fillText('เส้นชัย', finishLine.x + finishLine.width / 2, finishLine.y + 170);
            
            // More celebration emojis
            ctx.font = '25px Arial';
            ctx.fillText('🎯', finishLine.x + finishLine.width / 2, finishLine.y + 210);
            ctx.fillText('🎖️', finishLine.x + finishLine.width / 2, finishLine.y + 250);

            // Draw player 🏃‍♂️
            if (!game.invincible || Math.floor(Date.now() / 100) % 2 === 0) {
                // Shadow
                ctx.fillStyle = 'rgba(0, 0, 0, 0.2)';
                ctx.ellipse(player.x + player.width / 2, player.y + player.height + 2, player.width / 2, 5, 0, 0, Math.PI * 2);
                ctx.fill();
                
                // Body gradient
                const playerGradient = ctx.createLinearGradient(player.x, player.y, player.x, player.y + player.height);
                playerGradient.addColorStop(0, '#4169E1');
                playerGradient.addColorStop(1, '#0066CC');
                ctx.fillStyle = playerGradient;
                ctx.fillRect(player.x, player.y + 20, player.width, player.height - 20);
                
                // Body outline
                ctx.strokeStyle = '#003399';
                ctx.lineWidth = 2;
                ctx.strokeRect(player.x, player.y + 20, player.width, player.height - 20);
                
                // Head
                ctx.fillStyle = '#FFE0BD';
                ctx.fillRect(player.x + 5, player.y, 30, 20);
                ctx.strokeStyle = '#D4A574';
                ctx.strokeRect(player.x + 5, player.y, 30, 20);
                
                // Eyes
                ctx.fillStyle = '#000000';
                ctx.beginPath();
                ctx.arc(player.x + 13, player.y + 8, 2.5, 0, Math.PI * 2);
                ctx.fill();
                ctx.beginPath();
                ctx.arc(player.x + 27, player.y + 8, 2.5, 0, Math.PI * 2);
                ctx.fill();
                
                // Happy smile
                ctx.strokeStyle = '#000000';
                ctx.lineWidth = 2;
                ctx.beginPath();
                ctx.arc(player.x + 20, player.y + 10, 7, 0, Math.PI);
                ctx.stroke();
                
                // Cheeks
                ctx.fillStyle = 'rgba(255, 182, 193, 0.5)';
                ctx.beginPath();
                ctx.arc(player.x + 10, player.y + 12, 3, 0, Math.PI * 2);
                ctx.fill();
                ctx.beginPath();
                ctx.arc(player.x + 30, player.y + 12, 3, 0, Math.PI * 2);
                ctx.fill();
                
                // Running emoji on top
                ctx.font = '20px Arial';
                ctx.fillText('🏃', player.x + player.width / 2 - 10, player.y - 5);
                
                // Speed lines when moving
                if (Math.abs(player.velocityX) > 0) {
                    ctx.strokeStyle = 'rgba(255, 255, 255, 0.5)';
                    ctx.lineWidth = 2;
                    for (let i = 0; i < 3; i++) {
                        ctx.beginPath();
                        ctx.moveTo(player.x - 10 - i * 5, player.y + 20 + i * 10);
                        ctx.lineTo(player.x - 20 - i * 5, player.y + 20 + i * 10);
                        ctx.stroke();
                    }
                }
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
