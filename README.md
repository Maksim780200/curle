<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <title>Міні-Гра: Квадрати</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 20px;
        }

        #gameArea {
            width: 600px;
            height: 400px;
            border: 2px solid black;
            margin: 20px auto;
            position: relative;
            background: #f0f0f0;
        }

        .square {
            position: absolute;
            border-radius: 5px;
        }

        #player {
            width: 80px;
            height: 80px;
            background: dodgerblue;
            left: 50px;
            top: 150px;
            transition: 0.3s;
        }

        #food {
            width: 40px;
            height: 40px;
            background: orange;
            right: 50px;
            top: 150px;
        }

        button {
            padding: 10px 20px;
            margin: 10px;
            font-size: 17px;
            cursor: pointer;
        }
    </style>
</head>
<body>

<h1>Міні-гра: Їдящий квадрат</h1>

<div id="gameArea">
    <div id="player" class="square"></div>
    <div id="food" class="square"></div>
</div>

<button id="moveBtn">Рух до їжі</button>
<button id="splitBtn">Відділити квадрат</button>

<script>
    const player = document.getElementById("player");
    const food = document.getElementById("food");
    const area = document.getElementById("gameArea");

    let playerSize = 80;
    let foodExists = true;

    function getCoords(elem) {
        return elem.getBoundingClientRect();
    }

    // --- Рух до їжі ---
    document.getElementById("moveBtn").addEventListener("click", () => {
        if (!foodExists) return;

        // Отримуємо координати
        const playerRect = getCoords(player);
        const foodRect = getCoords(food);

        // Рухаємо гравця
        player.style.left = (food.offsetLeft - 20) + "px";
        player.style.top = (food.offsetTop - 20) + "px";

        setTimeout(() => {
            // Перевірка зіткнення
            if (Math.abs(playerRect.left - foodRect.left) < 100 &&
                Math.abs(playerRect.top - foodRect.top) < 100) {

                // З'їдання
                food.style.display = "none";
                foodExists = false;

                playerSize += 20;
                player.style.width = playerSize + "px";
                player.style.height = playerSize + "px";
            }
        }, 300);
    });

    // --- Відділення квадрата ---
    document.getElementById("splitBtn").addEventListener("click", () => {
        if (foodExists) return;

        // Відновити їжу
        food.style.display = "block";
        foodExists = true;

        // Нове положення маленького квадрата
        food.style.left = (player.offsetLeft + playerSize + 10) + "px";
        food.style.top = (player.offsetTop + 30) + "px";

        // Гравець зменшується
        if (playerSize > 40) {
            playerSize -= 20;
            player.style.width = playerSize + "px";
            player.style.height = playerSize + "px";
        }
    });
</script>

</body>
</html>
