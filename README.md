<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <title>Snake Game</title>
    <style>
        body {
            background: #222;
            color: white;
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 20px;
        }

        canvas {
            background: #111;
            border: 3px solid #00ff66;
            margin-top: 20px;
        }

        h1 {
            margin-bottom: 5px;
        }
    </style>
</head>
<body>

<h1>Змійка</h1>
<p>Управління: стрілки ↑ ↓ ← →</p>

<canvas id="game" width="400" height="400"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

// Розмір клітинки
const box = 20;

// Початкові координати змійки
let snake = [];
snake[0] = { x: 9 * box, y: 9 * box };

// Створення їжі
let food = {
    x: Math.floor(Math.random() * 20) * box,
    y: Math.floor(Math.random() * 20) * box
};

let direction;
let score = 0;

// Управління клавішами
document.addEventListener("keydown", setDirection);

function setDirection(event) {
    if (event.key === "ArrowLeft" && direction !== "RIGHT") {
        direction = "LEFT";
    } else if (event.key === "ArrowUp" && direction !== "DOWN") {
        direction = "UP";
    } else if (event.key === "ArrowRight" && direction !== "LEFT") {
        direction = "RIGHT";
    } else if (event.key === "ArrowDown" && direction !== "UP") {
        direction = "DOWN";
    }
}

// Головна функція гри
function drawGame() {
    // Малюємо фон
    ctx.fillStyle = "#222";
    ctx.fillRect(0, 0, 400, 400);

    // Малюємо їжу
    ctx.fillStyle = "red";
    ctx.fillRect(food.x, food.y, box, box);

    // Малюємо змійку
    for (let i = 0; i < snake.length; i++) {
        ctx.fillStyle = i === 0 ? "#00ff44" : "#00cc44";
        ctx.fillRect(snake[i].x, snake[i].y, box, box);
    }

    // Координати голови
    let snakeX = snake[0].x;
    let snakeY = snake[0].y;

    // Рух
    if (direction === "LEFT") snakeX -= box;
    if (direction === "RIGHT") snakeX += box;
    if (direction === "UP") snakeY -= box;
    if (direction === "DOWN") snakeY += box;

    // Якщо з'їв їжу
    if (snakeX === food.x && snakeY === food.y) {
        score++;
        food = {
            x: Math.floor(Math.random() * 20) * box,
            y: Math.floor(Math.random() * 20) * box
        };
    } else {
        snake.pop(); // видаляємо хвіст
    }

    // Нова голова
    let newHead = { x: snakeX, y: snakeY };

    // Перевірка удару в стіну або себе
    if (
        snakeX < 0 || snakeX >= 400 ||
        snakeY < 0 || snakeY >= 400 ||
        collision(newHead, snake)
    ) {
        alert("Гра закінчена! Твій результат: " + score);
        document.location.reload();
    }

    snake.unshift(newHead);

    // Вивід очок
    ctx.fillStyle = "white";
    ctx.font = "20px Arial";
    ctx.fillText("Очки: " + score, 10, 390);
}

// Перевірка на зіткнення хвоста з головою
function collision(head, arr) {
    for (let i = 0; i < arr.length; i++) {
        if (head.x === arr[i].x && head.y === arr[i].y) {
            return true;
        }
    }
    return false;
}

let game = setInterval(drawGame, 120);
</script>

</body>
</html>
