# Tic-tac-toe-web-application
A fully functional Tic-Tac-Toe game developed using HTML, CSS, and JavaScript.  Game Modes Winning Pattern Indication Responsive Design
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #1a1a1a;
            color: #f0f0f0;
        }

        .container {
            text-align: center;
        }

        h1 {
            color: #f0a500;
        }

        .game-board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-gap: 10px;
            margin: 20px auto;
        }

        .cell {
            width: 100px;
            height: 100px;
            background-color: #333;
            border: 2px solid #f0a500;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2em;
            color: #f0f0f0;
            cursor: pointer;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #f0a500;
            color: #1a1a1a;
            border: none;
            cursor: pointer;
            margin-top: 20px;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #d18b00;
        }

        #message {
            margin-top: 20px;
            font-size: 18px;
            color: #f0a500;
        }

        #winningPattern {
            margin-top: 10px;
            font-size: 16px;
            color: #f0f0f0;
        }

        #options {
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Tic-Tac-Toe</h1>
        
        <!-- Options for playing against the computer or with another player -->
        <div id="options">
            <button onclick="setGameMode('computer')">Play with Computer</button>
            <button onclick="setGameMode('player')">Play with Another Player</button>
        </div>

        <div id="gameBoard" class="game-board" style="display: none;">
            <div class="cell" id="cell0" onclick="makeMove(0)"></div>
            <div class="cell" id="cell1" onclick="makeMove(1)"></div>
            <div class="cell" id="cell2" onclick="makeMove(2)"></div>
            <div class="cell" id="cell3" onclick="makeMove(3)"></div>
            <div class="cell" id="cell4" onclick="makeMove(4)"></div>
            <div class="cell" id="cell5" onclick="makeMove(5)"></div>
            <div class="cell" id="cell6" onclick="makeMove(6)"></div>
            <div class="cell" id="cell7" onclick="makeMove(7)"></div>
            <div class="cell" id="cell8" onclick="makeMove(8)"></div>
        </div>
        <button id="resetBtn" onclick="resetGame()" style="display: none;">Restart Game</button>
        <p id="message"></p>
        <p id="winningPattern"></p>
    </div>

    <script>
        let board = ["", "", "", "", "", "", "", "", ""];
        let currentPlayer = "X"; // Player starts as "X"
        let isGameActive = true;
        let gameMode = ""; // Either 'computer' or 'player'
        const player = "X";
        const computer = "O";

        const winningConditions = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6]
        ];

        const messageElement = document.getElementById('message');
        const gameBoardElement = document.getElementById('gameBoard');
        const resetButton = document.getElementById('resetBtn');
        const optionsElement = document.getElementById('options');
        const winningPatternElement = document.getElementById('winningPattern');

        function setGameMode(mode) {
            gameMode = mode;
            gameBoardElement.style.display = 'grid';
            resetButton.style.display = 'block';
            optionsElement.style.display = 'none';
            messageElement.innerText = `Player ${currentPlayer}'s turn`;
        }

        function makeMove(index) {
            if (board[index] === "" && isGameActive) {
                board[index] = currentPlayer;
                document.getElementById(`cell${index}`).innerText = currentPlayer;
                checkWinner();
                if (isGameActive) {
                    switchPlayer();
                    if (currentPlayer === computer && gameMode === 'computer') {
                        setTimeout(computerMove, 500); // Delay to simulate thinking
                    }
                }
            }
        }

        function computerMove() {
            let availableCells = board
                .map((value, index) => (value === "" ? index : null))
                .filter(value => value !== null);

            if (availableCells.length > 0) {
                const randomIndex = availableCells[Math.floor(Math.random() * availableCells.length)];
                board[randomIndex] = computer;
                document.getElementById(`cell${randomIndex}`).innerText = computer;
                checkWinner();
                if (isGameActive) {
                    switchPlayer();
                }
            }
        }

        function switchPlayer() {
            currentPlayer = currentPlayer === player ? computer : player;
            if (isGameActive) {
                messageElement.innerText = `Player ${currentPlayer === player ? "X" : "O"}'s turn`;
            }
        }

        function checkWinner() {
            for (let i = 0; i < winningConditions.length; i++) {
                const [a, b, c] = winningConditions[i];
                if (board[a] && board[a] === board[b] && board[a] === board[c]) {
                    messageElement.innerText = `Player ${currentPlayer} wins! Click restart to play again!`;
                    winningPatternElement.innerText = `Winning Pattern: Cells ${a + 1}, ${b + 1}, and ${c + 1}`;
                    isGameActive = false;
                    return;
                }
            }

            if (!board.includes("")) {
                messageElement.innerText = "It's a draw! Click restart to play again!";
                winningPatternElement.innerText = ""; // No pattern on draw
                isGameActive = false;
            }
        }

        function resetGame() {
            board = ["", "", "", "", "", "", "", "", ""];
            isGameActive = true;
            currentPlayer = player; // Player starts as "X" again
            messageElement.innerText = `Player ${currentPlayer}'s turn`;
            winningPatternElement.innerText = ""; // Clear winning pattern
            document.querySelectorAll('.cell').forEach(cell => cell.innerText = "");
        }

        window.onload = () => {
            messageElement.innerText = "Choose game mode to start!";
        };
    </script>
</body>
</html>
