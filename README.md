<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>렌주룰 기반 오목 게임</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      margin: 0;
      padding: 0;
    }

    h1 {
      margin: 20px 0;
    }

    #board {
      display: grid;
      grid-template-columns: repeat(19, 40px);
      grid-template-rows: repeat(19, 40px);
      background-color: #deb887;
      border: 2px solid #8b4513;
      position: relative;
      width: 760px;
      height: 760px;
    }

    .cell {
      border: 1px solid #8b4513;
      position: relative;
      cursor: pointer;
    }

    .cell.black::after, .cell.white::after {
      content: "";
      position: absolute;
      top: 50%;
      left: 50%;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      transform: translate(-50%, -50%);
    }

    .cell.black::after {
      background-color: black;
    }

    .cell.white::after {
      background-color: white;
    }

    .intersection::after {
      content: "";
      position: absolute;
      top: 50%;
      left: 50%;
      width: 6px;
      height: 6px;
      background-color: black;
      border-radius: 50%;
      transform: translate(-50%, -50%);
    }

    #game-status {
      margin: 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <h1>렌주룰 기반 오목 게임</h1>
  <div id="board"></div>
  <p id="game-status">플레이어: 흑돌 차례</p>
  <script>
    const boardSize = 19;
    const board = Array.from({ length: boardSize }, () => Array(boardSize).fill(null));
    let currentPlayer = "black";
    let gameOver = false;

    const gameBoard = document.getElementById("board");
    const gameStatus = document.getElementById("game-status");

    // 바둑판 초기화
    function initializeBoard() {
      for (let row = 0; row < boardSize; row++) {
        for (let col = 0; col < boardSize; col++) {
          const cell = document.createElement("div");
          cell.classList.add("cell");
          cell.dataset.row = row;
          cell.dataset.col = col;

          // 주요 교차점 표시
          if ((row === 3 || row === 9 || row === 15) && (col === 3 || col === 9 || col === 15)) {
            cell.classList.add("intersection");
          }

          cell.addEventListener("click", handleMove);
          gameBoard.appendChild(cell);
        }
      }
    }

    // 턴 처리
    function handleMove(event) {
      if (gameOver) return;

      const row = parseInt(event.target.dataset.row);
      const col = parseInt(event.target.dataset.col);

      if (board[row][col]) {
        alert("이미 돌이 놓인 자리입니다!");
        return;
      }

      board[row][col] = currentPlayer;
      event.target.classList.add(currentPlayer);

      if (checkWin(row, col)) {
        gameStatus.textContent = `${currentPlayer === "black" ? "흑돌" : "백돌"} 승리!`;
        gameOver = true;
        return;
      }

      currentPlayer = currentPlayer === "black" ? "white" : "black";
      gameStatus.textContent = `플레이어: ${currentPlayer === "black" ? "흑돌" : "백돌"} 차례`;
    }

    // 승리 조건 확인
    function checkWin(row, col) {
      const directions = [
        [0, 1], [1, 0], [1, 1], [1, -1] // 우, 하, 대각선(우하), 대각선(좌하)
      ];

      for (const [dx, dy] of directions) {
        let count = 1;
        count += countStones(row, col, dx, dy);
        count += countStones(row, col, -dx, -dy);

        if (count >= 5) return true; // 5목 달성 시
      }
      return false;
    }

    // 돌 개수 세기
    function countStones(row, col, dx, dy) {
      let count = 0;
      let x = row + dx;
      let y = col + dy;

      while (x >= 0 && y >= 0 && x < boardSize && y < boardSize && board[x][y] === currentPlayer) {
        count++;
        x += dx;
        y += dy;
      }

      return count;
    }

    initializeBoard();
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>렌주룰 기반 오목 게임</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      font-family: Arial, sans-serif;
      background-color: #f4f4f4;
      margin: 0;
      padding: 0;
    }

    h1 {
      margin: 20px 0;
    }

    #board {
      display: grid;
      grid-template-columns: repeat(19, 40px);
      grid-template-rows: repeat(19, 40px);
      background-color: #deb887;
      border: 2px solid #8b4513;
      position: relative;
      width: 760px;
      height: 760px;
    }

    .cell {
      border: 1px solid #8b4513;
      position: relative;
      cursor: pointer;
    }

    .cell.black::after, .cell.white::after {
      content: "";
      position: absolute;
      top: 50%;
      left: 50%;
      width: 30px;
      height: 30px;
      border-radius: 50%;
      transform: translate(-50%, -50%);
    }

    .cell.black::after {
      background-color: black;
    }

    .cell.white::after {
      background-color: white;
    }

    .intersection::after {
      content: "";
      position: absolute;
      top: 50%;
      left: 50%;
      width: 6px;
      height: 6px;
      background-color: black;
      border-radius: 50%;
      transform: translate(-50%, -50%);
    }

    #game-status {
      margin: 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <h1>렌주룰 기반 오목 게임</h1>
  <div id="board"></div>
  <p id="game-status">플레이어: 흑돌 차례</p>
  <script>
    const boardSize = 19;
    const board = Array.from({ length: boardSize }, () => Array(boardSize).fill(null));
    let currentPlayer = "black";
    let gameOver = false;

    const gameBoard = document.getElementById("board");
    const gameStatus = document.getElementById("game-status");

    // 바둑판 초기화
    function initializeBoard() {
      for (let row = 0; row < boardSize; row++) {
        for (let col = 0; col < boardSize; col++) {
          const cell = document.createElement("div");
          cell.classList.add("cell");
          cell.dataset.row = row;
          cell.dataset.col = col;

          // 주요 교차점 표시
          if ((row === 3 || row === 9 || row === 15) && (col === 3 || col === 9 || col === 15)) {
            cell.classList.add("intersection");
          }

          cell.addEventListener("click", handleMove);
          gameBoard.appendChild(cell);
        }
      }
    }

    // 턴 처리
    function handleMove(event) {
      if (gameOver) return;

      const row = parseInt(event.target.dataset.row);
      const col = parseInt(event.target.dataset.col);

      if (board[row][col]) {
        alert("이미 돌이 놓인 자리입니다!");
        return;
      }

      board[row][col] = currentPlayer;
      event.target.classList.add(currentPlayer);

      if (checkWin(row, col)) {
        gameStatus.textContent = `${currentPlayer === "black" ? "흑돌" : "백돌"} 승리!`;
        gameOver = true;
        return;
      }

      currentPlayer = currentPlayer === "black" ? "white" : "black";
      gameStatus.textContent = `플레이어: ${currentPlayer === "black" ? "흑돌" : "백돌"} 차례`;
    }

    // 승리 조건 확인
    function checkWin(row, col) {
      const directions = [
        [0, 1], [1, 0], [1, 1], [1, -1] // 우, 하, 대각선(우하), 대각선(좌하)
      ];

      for (const [dx, dy] of directions) {
        let count = 1;
        count += countStones(row, col, dx, dy);
        count += countStones(row, col, -dx, -dy);

        if (count >= 5) return true; // 5목 달성 시
      }
      return false;
    }

    // 돌 개수 세기
    function countStones(row, col, dx, dy) {
      let count = 0;
      let x = row + dx;
      let y = col + dy;

      while (x >= 0 && y >= 0 && x < boardSize && y < boardSize && board[x][y] === currentPlayer) {
        count++;
        x += dx;
        y += dy;
      }

      return count;
    }

    initializeBoard();
  </script>
</body>
</html>
