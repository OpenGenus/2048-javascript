# 2048-javascript


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


CSS: With the Casacading Style Sheet(CSS) we will describe how HTML elements in the game will be dispalyed. The body will be styled giving it a background-color and padding-top. The score-container also will get a background-color for visiblity and also a specified height. Other styling will be applied to make the game colorful and give the game a good design.

JavaScript:
Here we will need to define a constants as a rule a constant is a type of variable whose value cannot be changed. Afterwards we define our variables.
Create the game board(canvas) and center the game board using centerCanvas. And we generate two(2) random values for X and Y coordinates.
Starting the game and for the purpose of suitability we are using 0 as empty cell which means at first the game field is empty.
After updating the the game grid with the correct tiles/tile colors, displays the score, and stops
the game if there are no moves left or the 2048 tile is achieved.

Thefunction ``keyPressed`` moves the tiles on the game board using functions: ``case UP_ARROW``, ``case DOWN_ARROW``, ``case RIGHT_ARROW`` and ``case LEFT_ARROW``.

If the game is won or over and the enter key is press, the game will refresh and proceed to start a new game.
The gameboard tiles are slide vertically to combine tiles of the similar values to collide and update the current column grid.


The gameboard tiles are slide horizontally to combine tiles of the similar values to collide and update the current column grid.


Then we will check that a tile is not empty and also check if after a key is press if a tile on the grid moved or if its false then the game is over.


We check here if there are any moves to play. This can be a result of an empty
tile or if two adjacent tiles have the same value. If the grid has empty spots then tile moves to the left. Also if a neighboring tile of the current tile has the same value, they are moved to the left.



The function combine will combine tiles of the same values together of a specific row in a given direction. And function ``combineDownRight`` does combine tiles of the same value together downwards or right. Then skip empty tiles until a nonempty tile or the beginning of the row is reached. The next code will combine and update score if the adjacent tiles have equal value. function ``combineUpLeft`` does combine tiles of the similar values upward or left. Omit empty tiles until a nonempty tile or the end of the row is reached. In a situation whereby the adjacent tiles have equal value, combine and update the score

Other notable functions:
function ``displayScore`` will display or insert

The function ``displayGameWon`` will display when the game is won(gameWon).It also uses the function displayText with a respective height and width.


The purpose of function ``displayText`` is to display texts on the canvas when called upon

Thefunction drawGrid is used to draw and style the grid. First we need to declare the variables. C is for the colors while ``GRID_SIZE`` determines the space between each squares. The color of tile will depends on value of tile

With this article at OpenGenus, you must have the complete idea of developing your own version of 2048 game using JavaScript.