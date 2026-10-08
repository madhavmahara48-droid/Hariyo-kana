<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Snake Game - Madhav Mahara</title>

  <!-- Bootstrap 5 -->
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
    rel="stylesheet"
  >

  <style>
    * {
      box-sizing: border-box;
      user-select: none;
    }

    body {
      margin: 0;
      min-height: 100vh;
      background: #080808;
      color: white;
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .game-wrapper {
      width: 100%;
      max-width: 520px;
      padding: 15px;
    }

    .game-card {
      background: #151515;
      border: 1px solid #333;
      border-radius: 25px;
      padding: 18px;
      box-shadow: 0 0 35px rgba(0, 0, 0, .7);
    }

    .game-title {
      font-weight: 800;
      letter-spacing: 2px;
    }

    .snake-icon {
      font-size: 45px;
    }

    .score-box {
      background: #222;
      border-radius: 12px;
      padding: 8px 14px;
      text-align: center;
    }

    .score-number {
      color: #ffc107;
      font-size: 22px;
      font-weight: bold;
    }

    /* Game Board */
    #gameBoard {
      width: 100%;
      aspect-ratio: 1 / 1;
      background:
        linear-gradient(#202020 1px, transparent 1px),
        linear-gradient(90deg, #202020 1px, transparent 1px);
      background-size: 20px 20px;
      border: 3px solid #ffc107;
      border-radius: 15px;
      position: relative;
      overflow: hidden;
      margin-top: 15px;
    }

    .snake {
      position: absolute;
      width: 5%;
      height: 5%;
      background: #28a745;
      border-radius: 6px;
      border: 1px solid #7dff91;
      z-index: 5;
    }

    .snake.head {
      background: #ffc107;
      border-radius: 8px;
    }

    .food {
      position: absolute;
      width: 5%;
      height: 5%;
      background: #dc3545;
      border-radius: 50%;
      box-shadow: 0 0 12px rgba(220, 53, 69, .8);
      z-index: 4;
    }

    /* Overlay */
    .overlay {
      position: absolute;
      inset: 0;
      background: rgba(0, 0, 0, .78);
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
      z-index: 20;
      border-radius: 12px;
    }

    .overlay-content h2 {
      color: #ffc107;
      font-weight: 800;
    }

    /* Controls */
    .controls {
      width: 220px;
      margin: 20px auto 5px;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 8px;
    }

    .control-btn {
      height: 58px;
      font-size: 25px;
      font-weight: bold;
      border-radius: 15px;
      touch-action: manipulation;
    }

    .empty {
      visibility: hidden;
    }

    .developer {
      color: #777;
      font-size: 13px;
    }

    @media (max-width: 400px) {
      .game-card {
        padding: 12px;
      }

      .control-btn {
        height: 52px;
      }
    }
  </style>
</head>

<body>

<div class="game-wrapper">

  <div class="game-card">

    <!-- Header -->
    <div class="text-center mb-2">
      <div class="snake-icon">🐍</div>

      <h2 class="game-title text-warning">
        SNAKE GAME
      </h2>
    </div>

    <!-- Score -->
    <div class="row g-2">

      <div class="col-6">
        <div class="score-box">
          <small>Score</small>
          <div id="score" class="score-number">0</div>
        </div>
      </div>

      <div class="col-6">
        <div class="score-box">
          <small>High Score</small>
          <div id="highScore" class="score-number">0</div>
        </div>
      </div>

    </div>

    <!-- Game Board -->
    <div id="gameBoard">

      <div id="overlay" class="overlay">

        <div class="overlay-content">

          <div class="display-1">🐍</div>

          <h2 id="overlayTitle">
            SNAKE GAME
          </h2>

          <p id="overlayMessage">
            Eat the red food and grow!
          </p>

          <button
            id="startBtn"
            class="btn btn-warning btn-lg px-4"
            onclick="startGame()">
            ▶ START GAME
          </button>

        </div>

      </div>

    </div>

    <!-- Mobile Controls -->
    <div class="controls">

      <button
        class="btn btn-warning control-btn empty">
      </button>

      <button
        class="btn btn-warning control-btn"
        onclick="changeDirection('up')">
        ↑
      </button>

      <button
        class="btn btn-warning control-btn empty">
      </button>

      <button
        class="btn btn-warning control-btn"
        onclick="changeDirection('left')">
        ←
      </button>

      <button
        class="btn btn-warning control-btn"
        onclick="changeDirection('down')">
        ↓
      </button>

      <button
        class="btn btn-warning control-btn"
        onclick="changeDirection('right')">
        →
      </button>

    </div>

    <!-- Buttons -->
    <div class="d-flex gap-2 mt-3">

      <button
        class="btn btn-outline-light flex-fill"
        onclick="togglePause()">
        ⏸ Pause
      </button>

      <button
        class="btn btn-outline-warning flex-fill"
        onclick="restartGame()">
        🔄 Restart
      </button>

    </div>

    <div class="text-center mt-3 developer">
      Developed by Madhav Mahara
    </div>

  </div>

</div>


<script>

  /* =========================
     GAME SETTINGS
  ========================= */

  const board = document.getElementById("gameBoard");

  const scoreText = document.getElementById("score");
  const highScoreText = document.getElementById("highScore");

  const overlay = document.getElementById("overlay");
  const overlayTitle = document.getElementById("overlayTitle");
  const overlayMessage = document.getElementById("overlayMessage");
  const startBtn = document.getElementById("startBtn");

  const GRID = 20;

  let snake = [];
  let food = {};

  let direction = "right";
  let nextDirection = "right";

  let score = 0;
  let highScore =
    Number(localStorage.getItem("snakeHighScore")) || 0;

  let gameRunning = false;
  let paused = false;

  let gameLoop = null;

  highScoreText.innerText = highScore;


  /* =========================
     START GAME
  ========================= */

  function startGame() {

    snake = [
      { x: 10, y: 10 },
      { x: 9, y: 10 },
      { x: 8, y: 10 }
    ];

    direction = "right";
    nextDirection = "right";

    score = 0;
    scoreText.innerText = score;

    paused = false;
    gameRunning = true;

    overlay.style.display = "none";

    createFood();
    draw();

    clearInterval(gameLoop);

    gameLoop = setInterval(updateGame, 120);
  }


  /* =========================
     UPDATE GAME
  ========================= */

  function updateGame() {

    if (!gameRunning || paused) {
      return;
    }

    direction = nextDirection;

    const head = {
      x: snake[0].x,
      y: snake[0].y
    };

    /* Move */
    if (direction === "up") {
      head.y--;
    }

    if (direction === "down") {
      head.y++;
    }

    if (direction === "left") {
      head.x--;
    }

    if (direction === "right") {
      head.x++;
    }


    /* Wall collision */

    if (
      head.x < 0 ||
      head.x >= GRID ||
      head.y < 0 ||
      head.y >= GRID
    ) {
      gameOver();
      return;
    }


    /* Snake collision */

    for (let i = 0; i < snake.length; i++) {

      if (
        head.x === snake[i].x &&
        head.y === snake[i].y
      ) {
        gameOver();
        return;
      }
    }


    snake.unshift(head);


    /* Eat food */

    if (
      head.x === food.x &&
      head.y === food.y
    ) {

      score++;

      scoreText.innerText = score;

      if (score > highScore) {

        highScore = score;

        highScoreText.innerText = highScore;

        localStorage.setItem(
          "snakeHighScore",
          highScore
        );
      }

      createFood();

    } else {

      snake.pop();

    }

    draw();
  }


  /* =========================
     DRAW SNAKE
  ========================= */

  function draw() {

    document
      .querySelectorAll(".snake, .food")
      .forEach(element => element.remove());


    snake.forEach((part, index) => {

      const element =
        document.createElement("div");

      element.className = "snake";

      if (index === 0) {
        element.classList.add("head");
      }

      element.style.left =
        (part.x * 5) + "%";

      element.style.top =
        (part.y * 5) + "%";

      board.appendChild(element);

    });


    /* Draw food */

    const foodElement =
      document.createElement("div");

    foodElement.className = "food";

    foodElement.style.left =
      (food.x * 5) + "%";

    foodElement.style.top =
      (food.y * 5) + "%";

    board.appendChild(foodElement);
  }


  /* =========================
     CREATE FOOD
  ========================= */

  function createFood() {

    let valid = false;

    while (!valid) {

      food = {
        x: Math.floor(Math.random() * GRID),
        y: Math.floor(Math.random() * GRID)
      };

      valid = !snake.some(part =>
        part.x === food.x &&
        part.y === food.y
      );
    }
  }


  /* =========================
     CHANGE DIRECTION
  ========================= */

  function changeDirection(newDirection) {

    if (!gameRunning) return;

    if (
      newDirection === "up" &&
      direction !== "down"
    ) {
      nextDirection = "up";
    }

    if (
      newDirection === "down" &&
      direction !== "up"
    ) {
      nextDirection = "down";
    }

    if (
      newDirection === "left" &&
      direction !== "right"
    ) {
      nextDirection = "left";
    }

    if (
      newDirection === "right" &&
      direction !== "left"
    ) {
      nextDirection = "right";
    }
  }


  /* =========================
     KEYBOARD CONTROLS
  ========================= */

  document.addEventListener("keydown", function(event) {

    if (
      event.key === "ArrowUp" ||
      event.key.toLowerCase() === "w"
    ) {
      changeDirection("up");
    }

    if (
      event.key === "ArrowDown" ||
      event.key.toLowerCase() === "s"
    ) {
      changeDirection("down");
    }

    if (
      event.key === "ArrowLeft" ||
      event.key.toLowerCase() === "a"
    ) {
      changeDirection("left");
    }

    if (
      event.key === "ArrowRight" ||
      event.key.toLowerCase() === "d"
    ) {
      changeDirection("right");
    }

    if (event.code === "Space") {
      togglePause();
    }

  });


  /* =========================
     PAUSE
  ========================= */

  function togglePause() {

    if (!gameRunning) return;

    paused = !paused;

    if (paused) {

      overlay.style.display = "flex";

      overlayTitle.innerText = "GAME PAUSED";

      overlayMessage.innerText =
        "Press Pause again to continue.";

      startBtn.innerText = "▶ RESUME";

      startBtn.onclick = togglePause;

    } else {

      overlay.style.display = "none";

      startBtn.onclick = startGame;

    }
  }


  /* =========================
     GAME OVER
  ========================= */

  function gameOver() {

    gameRunning = false;

    clearInterval(gameLoop);

    overlay.style.display = "flex";

    overlayTitle.innerText = "GAME OVER";

    overlayMessage.innerText =
      "Your Score: " + score;

    startBtn.innerText = "🔄 PLAY AGAIN";

    startBtn.onclick = restartGame;
  }


  /* =========================
     RESTART
  ========================= */

  function restartGame() {

    clearInterval(gameLoop);

    overlay.style.display = "none";

    startGame();
  }

</script>

</body>
</html>
