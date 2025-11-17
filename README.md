<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <title>Міні-гра: Змійка</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 20px;
        }

        #gameArea {
            width: 500px;
            height: 500px;
            border: 3px solid black;
            margin: 20px auto;
            background: #e8e8e8;
            position: relative;
            overflow: hidden;
        }

        .segment {
            width: 20px;
            height: 20px;
            background: dodgerblue;
            position: absolute;
        }

        #food {
            width: 20px;
            height: 20px;
            background: orange;
            position: absolute;
        }

        button {
            padding: 10px 20px;
            margin: 10px;
            font-size: 16px;
        }
    </style>
</head>
<body>

<h1>Міні-гра: Змійка</h1>

<div id="gameArea">
    <div id="food"></div>
</div>

<button onclick="changeDirection('up')">Вгору</button>
<button onclick="changeDirection('left')">Вліво</button>
<button onclick="changeDirection('right')">Вправо</button>
<button onclick="changeDirection('down')">Вниз</button>
<br>
<button onclick="splitTail()">Відділити квадратик</button>

<script>
const area = document.getElementById("gameArea");
const food = document.getElementById("food");

// Положення їжі
function placeFood() {
    food.style.left = Math.floor(Math.random() * 25) * 20 + "px";
    food.style.top = Math.floor(Math.random() * 25) * 20 + "px";
}
placeFood();

// Масив сегментів змійки
let snake = [
    {x: 200, y: 200}, // голова
];

let direction = "right";

// Створюємо HTML елемент для голови
function drawSnake() {
    area.innerHTML = "";
    area.appendChild(food);

    snake.forEach(seg => {
        let div = document.createElement("div");
        div.className = "segment";
        div.style.left = seg.x + "px";
        div.style.top = seg.y + "px";
        area.appendChild(div);
    });
}

drawSnake();

// рух змійки
function move() {
    let head = {...snake[0]};

    if (direction === "right") head.x += 20;
    if (direction === "left") head.x -= 20;
    if (direction === "up") head.y -= 20;
    if (direction === "down") head.y += 20;

    // додаємо нову голову
    snake.unshift(head);

    // Перевірка на з'їдання
    if (head.x == parseInt(food.style.left) &&
        head.y == parseInt(food.style.top)) {
        
        placeFood(); // нова їжа
    } else {
        snake.pop(); // рух: прибираємо хвіст
    }

    drawSnake();
}

// зміна напрямку
function changeDirection(dir) {
    direction = dir;
}

// відділити частину (зменшити змійку)
function splitTail() {
    if (snake.length > 1) {
        snake.pop();  // відривається один сегмент
        drawSnake();
    }
}

// запуск гри
setInterval(move, 150);
</script>

</body>
</html>
