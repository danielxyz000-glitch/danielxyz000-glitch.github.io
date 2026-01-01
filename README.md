<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Крестики-нолики</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 50px;
        }

        h1 {
            margin-bottom: 20px;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
        }

        .cell {
            width: 100px;
            height: 100px;
            font-size: 48px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 2px solid #333;
            cursor: pointer;
            user-select: none;
        }

        .cell:hover {
            background-color: #f0f0f0;
        }

        button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>

<h1>Крестики-нолики</h1>

<div class="board" id="board"></div>

<button onclick="resetGame()">Новая игра</button>

<script>
    const boardElement = document.getElementById("board");
    let board = Array(9).fill("");
    let currentPlayer = "X";

    const winCombinations = [
        [0,1,2], [3,4,5], [6,7,8],
        [0,3,6], [1,4,7], [2,5,8],
        [0,4,8], [2,4,6]
    ];

    function checkWinner(player) {
        return winCombinations.some(combo =>
            combo.every(index => board[index] === player)
        );
    }

    function isDraw() {
        return board.every(cell => cell !== "");
    }

    function handleClick(index) {
        if (board[index] !== "") return;

        board[index] = currentPlayer;
        render();

        if (checkWinner(currentPlayer)) {
            setTimeout(() => alert(`Игрок ${currentPlayer} победил!`), 10);
            resetGame();
            return;
        }

        if (isDraw()) {
            setTimeout(() => alert("Ничья!"), 10);
            resetGame();
            return;
        }

        currentPlayer = currentPlayer === "X" ? "O" : "X";
    }

    function render() {
        boardElement.innerHTML = "";
        board.forEach((cell, index) => {
            const div = document.createElement("div");
            div.className = "cell";
            div.textContent = cell;
            div.onclick = () => handleClick(index);
            boardElement.appendChild(div);
        });
    }

    function resetGame() {
        board = Array(9).fill("");
        currentPlayer = "X";
        render();
    }

    render();
</script>

</body>
</html>
