# 2048-javascript

<img src="C:\Users\HP\Desktop\2048-javascript\images\funsho2048.jpg" alt="Alt text" title="Optional title">

In this article, we have demonstrated how to develop web version of 2048 game using JavaScript, HTML and CSS. This is a good project for a Web Developer Portfolio.

Table of Contents:

Introduction to 2048 game
The Composition / Development
Introduction to 2048 game
2048 is a single-player sliding tile puzzle video game written by Italian web developer Gabriele Cirulli and published on GitHub. The objective of the game is to slide numbered tiles on a grid to combine them to create a tile with the number 2048.

2048 game is a single-player game where the target is to form the number 2048. Tiles with a matching values (number) can be merged into a single tile and receives the values sum. The game can be played using PC controls (Keyboard) and on mobile. You can also view the scores.

The Composition / Development
The 2048 Game is built using HTML, CSS, and JavaScript. The game player has to slide the numbers to merge them into larger numbers. The number with the same values can only merge to form a larger number of their sum (i.e: 2+2 =4). To move the tiles on PC you will use the directional keys: ⥣ ⥢ ⥤ ⥥. On a mobile or tablet you can simply swipe.

Done

Grid created in HTML CSS
Add EventListener on arrow keys
Generate new tile
Board starts automatically with 2 random numbers.
Score improvements needed when update is done.
Refactor the move() function
HTML: 2048 game is built using JavaScript without any libraries, but the first step is to create the HTML template. The game borad will be created using HTML table. The first step in creating the game is to define HTML which is the code to structure the gameboard. We create a div with class="container". A div also for score-container another for score-title and score. Lastly a paragraph for game instructions. Its important to note that all rows and cells will be designed through JavaScript functions. Next we will style the game board using CSS.

```<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>2048 Game Using JavaScript</title>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.6.0/p5.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.6.0/addons/p5.dom.min.js"></script>
        <link href="style.css" rel="stylesheet">
    </head>
    <body>
        <div class="container">
            <h1 id="title">2048</h1>
            <div id="score-container">
                <div id="score-title">SCORE</div>
                <div id="score"></div>
            </div>
            <p id="instructions">Hit the arrow keys to slide the tiles and reach the <span>2048</span> tile!</p>
        </div>
        <script src="script.js"></script>
    </body>
</html>```


CSS: With the Casacading Style Sheet(CSS) we will describe how HTML elements in the game will be dispalyed. The body will be styled giving it a background-color and padding-top. The score-container also will get a background-color for visiblity and also a specified height. Other styling will be applied to make the game colorful and give the game a good design.

```body {
    background-color: #faf8ef;
    color: #776E65;
    font-family: Arial, Helvetica, sans-serif;
    padding-top: 2%;
}


.container {
    width: 450px;
    margin-left: auto;
    margin-right: auto;
}


h1#title {
    font-size: 80px;
    font-weight: bold;
    margin: 0;
    display: block;
    float: left;
}

/
div#score-container {
    position: relative;
    float: right;
    background-color: #776E65;
    color: rgb(205, 192, 180);
    height: 25px;
    padding: 15px 20px;
    padding-bottom: 30px;
    margin-top: 10px;
    text-align: center;
}


div#score {
    font-weight: bold;
    font-size: 25px;
    color: white;
}

p#instructions {
    position: relative;
    float: left;
}


span {
    font-weight: bold;
}
```

JavaScript:
Here we will need to define a constants as a rule a constant is a type of variable whose value cannot be changed. Afterwards we define our variables.

```const GRID_SIZE = 4;
const CELL_SIZE = 100;


var canvas;
var grid;
var gameOver;
var score;
var gameWon;```


Create the game board(canvas) and center the game board using centerCanvas. And we generate two(2) random values for X and Y coordinates.

```function setup() {
    canvas = createCanvas(GRID_SIZE * CELL_SIZE + 50, GRID_SIZE * CELL_SIZE + 50);
    background(187, 173, 160);
    centerCanvas(canvas);
    newGame();
    noLoop();
    updateGrid();
}

function centerCanvas() {
    var x = (windowWidth - width) / 2;
    var y = (windowHeight - height) / 2 + 40;
    canvas.position(x, y);
}```

Starting the game and for the purpose of suitability we are using 0 as empty cell which means at first the game field is empty.

```grid = new Array(GRID_SIZE * GRID_SIZE).fill(0);
    gameOver = false;
    gameWon = false;
    score = 0;
    //add the two starting tiles
    addRandomTile();
    addRandomTile();
}```

After updating the the game grid with the correct tiles/tile colors, displays the score, and stops
the game if there are no moves left or the 2048 tile is achieved.

```function updateGrid() {
    displayScore();
    drawGrid();
    if (gameOver) {
        displayGameOver();
    }
    else if (gameWon) {
        displayGameWon();
    }
}```

Thefunction ``keyPressed`` moves the tiles on the game board using functions: ``case UP_ARROW``, ``case DOWN_ARROW``, ``case RIGHT_ARROW`` and ``case LEFT_ARROW``.

```function keyPressed() {
    if (!gameOver && !gameWon) {
        switch (keyCode) {
            case UP_ARROW:
                verticalSlide(keyCode);
                updateGrid();
                break;
            case DOWN_ARROW:
                verticalSlide(keyCode);
                updateGrid();
                break;
            case RIGHT_ARROW:
                horizontalSlide(keyCode);
                updateGrid();
                break;
            case LEFT_ARROW:
                horizontalSlide(keyCode);
                updateGrid();
                break;
        }
    }```

If the game is won or over and the enter key is press, the game will refresh and proceed to start a new game.

```else if (keyCode === ENTER) {
        if (gameWon) {
            location.reload();
        }
        else {
            location.reload();
        }
    }
}```

The gameboard tiles are slide vertically to combine tiles of the similar values to collide and update the current column grid.

```function verticalSlide(direction) {
    var previousGrid = [];
    var column;
    var filler;

    arrayCopy(grid, previousGrid);

    for (var i = 0; i < GRID_SIZE; i++) {
        column = [];
        for (var j = i; j < GRID_SIZE * GRID_SIZE; j += 4) {
            column.push(grid[j]);
        }

        column = combine(column, direction);

        column = column.filter(notEmpty);

        filler = new Array(GRID_SIZE - column.length).fill(0);
        if (direction === UP_ARROW) {
            column = column.concat(filler);
        }
        else {
            column = filler.concat(column);
        }


        for (var k = 0; k < column.length; k++) {
            grid[k * GRID_SIZE + i] = column[k];
        }

       
        combine(column, direction);
    }
    checkSlide(previousGrid);
}```

The gameboard tiles are slide horizontally to combine tiles of the similar values to collide and update the current column grid.

```function horizontalSlide(direction) {
    var previousGrid = [];
    var row;
    var filler;

    arrayCopy(grid, previousGrid);

    for (var i = 0; i < GRID_SIZE; i++) {

        row = grid.slice(i * GRID_SIZE, i * GRID_SIZE + GRID_SIZE);

        row = combine(row, direction);

        row = row.filter(notEmpty);

        filler = new Array(GRID_SIZE - row.length).fill(0);
        if (direction === LEFT_ARROW) {
            row = row.concat(filler);
        }
        else {
            row = filler.concat(row);
        }
    grid.splice(i * GRID_SIZE, GRID_SIZE);
        grid.splice(i * GRID_SIZE, 0, ...row);
    }
    checkSlide(previousGrid);
}```

Then we will check that a tile is not empty and also check if after a key is press if a tile on the grid moved or if its false then the game is over.

```function notEmpty(x) {
    return x > 0;
}

function checkSlide(previousGrid) {
    if (!(grid.every((x, i) => x === previousGrid[i]))) {
        addRandomTile();
    }
    if (!movesLeft()) {
        gameOver = true;
    }
}```


We check here if there are any moves to play. This can be a result of an empty
tile or if two adjacent tiles have the same value. If the grid has empty spots then tile moves to the left. Also if a neighboring tile of the current tile has the same value, they are moved to the left.

```function movesLeft() {
    var movesLeft = false;
    var flag = false;
    var currentTile;
    var right;
    var bottom;

    for (var i = 0; i < GRID_SIZE; i++) {
        for (var j = 0; j < GRID_SIZE; j++) {
            if (!flag) {
                currentTile = grid[i * GRID_SIZE + j];

               
                if (currentTile === 0) {
                    movesLeft = true;
                    flag = true;
                }
                else {
                    if (j < GRID_SIZE - 1) {
                        right = grid[i * GRID_SIZE + j + 1];
                    }
                    else {
                        right = 0;
                    }
                    if (i < GRID_SIZE - 1) {
                        bottom = grid[(i + 1) * GRID_SIZE + j];
                    }
                    else {
                        bottom = 0;
                    }
 if (currentTile === right || currentTile === bottom) {
                        movesLeft = true;
                        flag = true;
                    }
                }
            }
        }
    }```

The function combine will combine tiles of the same values together of a specific row in a given direction. And function ``combineDownRight`` does combine tiles of the same value together downwards or right. Then skip empty tiles until a nonempty tile or the beginning of the row is reached. The next code will combine and update score if the adjacent tiles have equal value. function ``combineUpLeft`` does combine tiles of the similar values upward or left. Omit empty tiles until a nonempty tile or the end of the row is reached. In a situation whereby the adjacent tiles have equal value, combine and update the score

```function combine(row, direction) {
    switch (direction) {
        case DOWN_ARROW:
            row = combineDownRight(row);
            break;
        case RIGHT_ARROW:
            row = combineDownRight(row);
            break;
        case UP_ARROW:
            row = combineUpLeft(row);
            break;
        case LEFT_ARROW:
            row = combineUpLeft(row);
            break;
    }

    return row;
}
function combineDownRight(row) {
    var x;
    var y;

    for (var i = row.length - 1; i > 0; i--) {
        //get current and subseqent tiles
        x = row[i];
        index = i - 1;
        y = row[index];

while (y === 0 && index > 0) {
            y = row[index--];
        }

   if (x === y && x !== 0) {
            row[i] = x + y;
            score += row[i];
            row[index] = 0;
            if (row[i] === 2048) {
                gameWon = true;
            }
        }
    }

    return row;
}

function combineUpLeft(row) {
    var x;
    var y;

    for (var i = 0; i < row.length - 1; i++) {
        //get current and subsequent tiles
        x = row[i];
        index = i + 1;
        y = row[index];
   while (y === 0 && index < row.length - 1) {
            y = row[index++];
        }
   if (x === y && x !== 0) {
            row[i] = x + y;
            score += row[i];
            row[index] = 0;
            if (row[i] === 2048) {
                gameWon = true;
            }
        }
    }

    return row;
}```

Thefunction ``addRandomTile`` add a tile with value 2 or 4 to an empty spot on the game board in a random manner. Then we add the indices of all empty tiles to the emptyTiles array

```function addRandomTile() {
    var emptyTiles = [];
    var index;
    var newTile = [2, 4];

    grid.forEach(function(value, index) {
        if (value === 0) {
            emptyTiles.push(index);
        }
    });
 if (emptyTiles.length > 0) {
        index = emptyTiles[Math.floor(Math.random() * emptyTiles.length)];
        grid[index] = newTile[Math.floor(Math.random() * newTile.length)];
    }
}``

Other notable functions:
function ``displayScore`` will display or insert

```function displayScore() {
    scoreContainer.innerHTML = `${score}`;
}```

The function ``displayGameOver`` displays the Game Over message on the board through the function displayText with a respective height and width.

```function displayGameOver() {
    displayText("Game Over!\nHit Enter to Play Again", color(119, 110, 101), 32, width / 2, height / 2);
}```

The function ``displayGameWon`` will display when the game is won(gameWon).It also uses the function displayText with a respective height and width.

```function displayGameWon() {
    displayText("You Win!\nHit Enter to Play Again", color(119, 110, 101), 32, width / 2, height / 2);
}```

The purpose of function ``displayText`` is to display texts on the canvas when called upon

```function displayText(message, color, size, xpos, ypos) {
    textAlign(CENTER);
    textSize(size);
    fill(color);
    text(message, xpos, ypos);
}```

Thefunction drawGrid is used to draw and style the grid. First we need to declare the variables. C is for the colors while ``GRID_SIZE`` determines the space between each squares. The color of tile will depends on value of tile

```function drawGrid() {
    var c;

    for (var i = 0; i < GRID_SIZE; i++) {
        for (var j = 0; j < GRID_SIZE; j++) {
            switch (grid[i * GRID_SIZE + j]) {
                case 0:
                    c = color("#CDC0B4");
                    break;
                case 2:
                    c = color("#EEE4DA");
                    break;
                case 4:
                    c = color("#EDE0C8");
                    break;
                case 8:
                    c = color("#B2DFDB");
                    break;
                case 16:
                    c = color("#80CBC4");
                    break;
                case 32:
                    c = color("#4DB6AC");
                    break;
                case 64:
                    c = color("#26A69A");
                    break;
                case 128:
                    c = color("#009688");
                    break;
                case 256:
                    c = color("#00897B");
                    break;
                case 512:
                    c = color("#00796B");
                    break;
                case 1024:
                    c = color("#00695C");
                    break;
                case 2048:
                    c = color("#004D40");
                    break;
            }

            fill(c);
            strokeWeight(2);

            stroke(c); 

            rect(j * CELL_SIZE + j * 10 + 10, i * CELL_SIZE + i * 10 + 10, CELL_SIZE, CELL_SIZE, 5);

            if (grid[i * GRID_SIZE + j] !== 0) {
                displayText(`${grid[i * GRID_SIZE + j]}`,
                color(255, 255, 255),
                45,
                j * CELL_SIZE + j * 10 + 10 + CELL_SIZE / 2,
                i * CELL_SIZE + i * 10 + 20 + CELL_SIZE / 2);
            }
        }
    }
}```




With this article at OpenGenus, you must have the complete idea of developing your own version of 2048 game using JavaScript.