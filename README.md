# danyang.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2048</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f2f2f2;
        }
        .game-container {
            width: 400px;
            height: 400px;
            border: 10px solid #bbada0;
            border-radius: 10px;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-template-rows: repeat(4, 1fr);
            gap: 10px;
            background-color: #f2f2f2;
        }
        .tile {
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 32px;
            font-weight: bold;
            color: #776e65;
            background-color: #f2f2f2;
            border-radius: 5px;
        }
        .tile-2 { background-color: #eee4da; }
        .tile-4 { background-color: #ede0c8; }
        .tile-8 { background-color: #f2b179; }
        .tile-16 { background-color: #f59563; }
        .tile-32 { background-color: #f67c5f; }
        .tile-64 { background-color: #f65e3b; }
        .tile-128 { background-color: #edcf72; }
        .tile-256 { background-color: #edcc61; }
        .tile-512 { background-color: #edc850; }
        .tile-1024 { background-color: #edc53f; }
        .tile-2048 { background-color: #edc22e; }
    </style>
</head>
<body>
    <div class="game-container" id="game-container"></div>

    <script>
        const boardSize = 4;
        const gameContainer = document.getElementById('game-container');
        let board = [];

        // 初始化棋盘
        function initBoard() {
            board = Array.from({ length: boardSize }, () => Array(boardSize).fill(0));
            addNewTile();
            addNewTile();
            renderBoard();
        }

        // 添加新方块
        function addNewTile() {
            const emptyCells = [];
            for (let i = 0; i < boardSize; i++) {
                for (let j = 0; j < boardSize; j++) {
                    if (board[i][j] === 0) {
                        emptyCells.push({ row: i, col: j });
                    }
                }
            }
            if (emptyCells.length === 0) return;
            const randomCell = emptyCells[Math.floor(Math.random() * emptyCells.length)];
            board[randomCell.row][randomCell.col] = Math.random() > 0.9 ? 4 : 2;
        }

        // 渲染棋盘
        function renderBoard() {
            gameContainer.innerHTML = '';
            for (let i = 0; i < boardSize; i++) {
                for (let j = 0; j < boardSize; j++) {
                    const tile = document.createElement('div');
                    tile.classList.add('tile');
                    if (board[i][j] !== 0) {
                        tile.classList.add(`tile-${board[i][j]}`);
                        tile.textContent = board[i][j];
                    }
                    gameContainer.appendChild(tile);
                }
            }
        }

        // 检查游戏是否结束
        function isGameOver() {
            for (let i = 0; i < boardSize; i++) {
                for (let j = 0; j < boardSize; j++) {
                    if (board[i][j] === 0) return false;
                    if (i !== 0 && board[i][j] === board[i - 1][j]) return false;
                    if (i !== boardSize - 1 && board[i][j] === board[i + 1][j]) return false;
                    if (j !== 0 && board[i][j] === board[i][j - 1]) return false;
                    if (j !== boardSize - 1 && board[i][j] === board[i][j + 1]) return false;
                }
            }
            return true;
        }

        // 合并方块
        function mergeTiles(direction) {
            let hasMerged = false;
            switch (direction) {
                case 'up':
                    for (let j = 0; j < boardSize; j++) {
                        for (let i = 1; i < boardSize; i++) {
                            if (board[i][j] !== 0) {
                                for (let k = i - 1; k >= 0; k--) {
                                    if (board[k][j] === 0) {
                                        board[k][j] = board[i][j];
                                        board[i][j] = 0;
                                        hasMerged = true;
                                    } else if (board[k][j] === board[i][j] && board[k + 1][j] !== board[i][j]) {
                                        board[k][j] *= 2;
                                        board[i][j] = 0;
                                        hasMerged = true;
                                        break;
                                    } else {
                                        break;
                                    }
                                }
                            }
                        }
                    }
                    break;
                case 'down':
                    for (let j = 0; j < boardSize; j++) {
                        for (let i = boardSize - 2; i >= 0; i--) {
                            if (board[i][j] !== 0) {
                                for (let k = i + 1; k < boardSize; k++) {
                                    if (board[k][j] === 0) {
                                        board[k][j] = board[i][j];
                                        board[i][j] = 0;
                                        hasMerged = true;
                                    } else if (board[k][j] === board[i][j] && board[k - 1][j] !== board[i][j]) {
                                        board[k][j] *= 2;
                                        board[i][j] = 0;
                                        hasMerged = true;
                                        break;
                                    } else {
                                        break;
                                    }
                                }
                            }
                        }
                    }
                    break;
                case 'left':
                    for (let i = 0; i < boardSize; i++) {
                        for (let j = 1; j < boardSize; j++) {
                            if (board[i][j] !== 0) {
                                for (let k = j - 1; k >= 0; k--) {
                                    if (board[i][k] === 0) {
                                        board[i][k] = board[i][j];
                                        board[i][j] = 0;
                                        hasMerged = true;
                                    } else if (board[i][k] === board[i][j] && board[i][k + 1] !== board[i][j]) {
                                        board[i][k] *= 2;
                                        board[i][j] = 0;
                                        hasMerged = true;
                                        break;
                                    } else {
                                        break;
                                    }
                                }
                            }
                        }
                    }
                    break;
                case 'right':
                    for (let i = 0; i < boardSize; i++) {
                        for (let j = boardSize - 2; j >= 0; j--) {
                            if (board[i][j] !== 0) {
                                for (let k = j + 1; k < boardSize; k++) {
                                    if (board[i][k] === 0) {
                                        board[i][k] = board[i][j];
                                        board[i][j] = 0;
                                        hasMerged = true;
                                    } else if (board[i][k] === board[i][j] && board[i][k - 1] !== board[i][j]) {
                                        board[i][k] *= 2;
                                        board[i][j] = 0;
                                        hasMerged = true;
                                        break;
                                    } else {
                                        break;
                                    }
                                }
                            }
                        }
                    }
                    break;
            }
            if (hasMerged) {
                addNewTile();
                renderBoard();
                if (isGameOver()) {
                    alert('游戏结束！');
                }
            }
        }

        // 监听键盘事件
        document.addEventListener('keydown', (event) =>
