
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Treasure Hunt Game</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            text-align: center;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            color: white;
            margin: 0;
            padding: 20px;
        }
        h1 {
            font-size: 36px;
            margin-bottom: 20px;
            text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
        }
        #game {
            display: grid;
            gap: 10px;
            margin: 20px auto;
            justify-content: center;
        }
        .cell {
            width: 60px;
            height: 60px;
            background-color: rgba(255, 255, 255, 0.1);
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 20px;
            transition: background-color 0.3s, transform 0.2s;
        }
        .cell:hover {
            background-color: rgba(255, 255, 255, 0.2);
            transform: scale(1.1);
        }
        .cell.clicked {
            background-color: #ffcc00;
        }
        .cell.treasure {
            background-color: #4caf50;
            animation: treasureFound 0.5s ease-in-out;
        }
        @keyframes treasureFound {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
        #level {
            font-size: 24px;
            margin-bottom: 20px;
        }
        #message {
            font-size: 20px;
            margin-top: 20px;
            color: #ffcc00;
        }
        #timer {
            font-size: 24px;
            margin-bottom: 20px;
        }
        #score {
            font-size: 24px;
            margin-bottom: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 18px;
            background-color: #4caf50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <h1>Treasure Hunt Game</h1>
    <div id="level">Level: 1</div>
    <div id="timer">Time Left: 30</div>
    <div id="score">Score: 0</div>
    <div id="game"></div>
    <div id="message"></div>
    <button onclick="resetGame()">Reset Game</button>

    <script>
        let level = 1;
        let gridSize = 3;
        let treasureLocation;
        let timeLeft = 30;
        let score = 0;
        let timerInterval;
        const game = document.getElementById('game');
        const levelDisplay = document.getElementById('level');
        const timerDisplay = document.getElementById('timer');
        const scoreDisplay = document.getElementById('score');
        const message = document.getElementById('message');

        // Function to start the game
        function startGame() {
            message.textContent = '';
            game.innerHTML = '';
            gridSize = level + 2; // Increase grid size with level
            game.style.gridTemplateColumns = `repeat(${gridSize}, 60px)`;
            treasureLocation = Math.floor(Math.random() * gridSize * gridSize);

            for (let i = 0; i < gridSize * gridSize; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                cell.addEventListener('click', () => handleClick(i));
                game.appendChild(cell);
            }

            startTimer();
        }

        // Function to handle cell click
        function handleClick(index) {
            const cell = game.children[index];
            if (index === treasureLocation) {
                cell.classList.add('treasure');
                message.textContent = 'You found the treasure!';
                score += timeLeft * 10; // Bonus points for remaining time
                scoreDisplay.textContent = `Score: ${score}`;
                clearInterval(timerInterval);
                setTimeout(() => {
                    level++;
                    if (level > 20) {
                        alert(`Congratulations! You completed all 20 levels! Final Score: ${score}`);
                        resetGame();
                    } else {
                        levelDisplay.textContent = `Level: ${level}`;
                        startGame();
                    }
                }, 1000);
            } else {
                cell.classList.add('clicked');
                message.textContent = 'Keep searching...';
            }
        }

        // Function to start the timer
        function startTimer() {
            timeLeft = 30;
            timerDisplay.textContent = `Time Left: ${timeLeft}`;
            clearInterval(timerInterval);
            timerInterval = setInterval(() => {
                timeLeft--;
                timerDisplay.textContent = `Time Left: ${timeLeft}`;
                if (timeLeft === 0) {
                    clearInterval(timerInterval);
                    alert('Time’s up! Try again.');
                    resetGame();
                }
            }, 1000);
        }

        // Function to reset the game
        function resetGame() {
            level = 1;
            score = 0;
            levelDisplay.textContent = `Level: ${level}`;
            scoreDisplay.textContent = `Score: ${score}`;
            startGame();
        }

        // Start the game
        startGame();
    </script>
</body>
</html>
