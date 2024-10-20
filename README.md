# Vanilla 2048 Game

A simple and interactive implementation of the classic 2048 game, built entirely with HTML, CSS, and vanilla JavaScript. This project is designed for educational purposes to help learners understand basic game logic, DOM manipulation, and JavaScript functions.

## Features
- **Classic 2048 Gameplay**: Move tiles and combine them to reach the elusive 2048 tile.
- **Smooth Animations**: Tiles slide and combine with smooth transitions.
- **Score Tracking**: Track your current score as you play.
- **Game Over & Restart Options**: Clear indication when the game is over and a button to restart.

## How to Play
Use the arrow keys to move the tiles:
- **Up Arrow**: Move tiles up.
- **Down Arrow**: Move tiles down.
- **Left Arrow**: Move tiles left.
- **Right Arrow**: Move tiles right.

Combine tiles with the same value to create larger tiles and increase your score. The game ends when there are no possible moves left.

## How to Run
1. Clone or download this repository.
2. Open `index.html` in your browser.

## File Structure
- `index.html`: The main HTML file that includes the structure of the game.
- `css/styles.css`: Contains all styles used for the game.
- `js/main.js`: Contains the core JavaScript code responsible for game logic and interactions.

## JavaScript Code Walkthrough
Below is a detailed explanation of how each major function works to help you understand the game logic:

### 1. `initializeBoard()`
This function initializes the game board by creating a 4x4 grid of zeros and adds two initial tiles to the board:
```javascript
function initializeBoard() {
  board = Array(boardSize).fill().map(() => Array(boardSize).fill(0));
  score = 0;
  addNewTile();
  addNewTile();
}
```
- **Purpose**: Sets up the game board and adds the initial tiles.
- **Key Concept**: Uses `Array.fill()` to create the grid.

### 2. `addNewTile()`
Adds a new tile of value `2` or `4` to a random empty position on the board:
```javascript
function addNewTile() {
  let emptyTiles = [];
  for (let i = 0; i < boardSize; i++) {
    for (let j = 0; j < boardSize; j++) {
      if (board[i][j] === 0) {
        emptyTiles.push({ x: i, y: j });
      }
    }
  }

  if (emptyTiles.length > 0) {
    let { x, y } = emptyTiles[Math.floor(Math.random() * emptyTiles.length)];
    board[x][y] = Math.random() < 0.9 ? 2 : 4;
  }
}
```
- **Purpose**: Adds a new tile to an empty spot, with 90% chance of being `2` and 10% chance of being `4`.
- **Key Concept**: Uses randomization to decide both the position and value of the new tile.

### 3. `drawBoard()`
Renders the current state of the board in the DOM:
```javascript
function drawBoard() {
  const gameBoard = document.getElementById('game-board');
  gameBoard.innerHTML = '';

  for (let i = 0; i < boardSize; i++) {
    for (let j = 0; j < boardSize; j++) {
      const tileValue = board[i][j];
      if (tileValue > 0) {
        const tile = document.createElement('div');
        tile.className = 'tile';
        tile.textContent = tileValue;
        tile.classList.add(`tile-${tileValue}`);
        tile.style.transform = `translate(${j * 100}px, ${i * 100}px)`;
        gameBoard.appendChild(tile);
      }
    }
  }
  document.getElementById('score').textContent = `Score: ${score}`;

  if (isGameOver()) {
    document.getElementById('game-over-message').style.display = 'block';
    document.getElementById('restart-button').style.display = 'block';
  }
}
```
- **Purpose**: Updates the UI to reflect the current state of the board and score.
- **Key Concept**: DOM manipulation and positioning tiles using CSS transforms.

### 4. `handleInput(event)`
Handles user input from arrow keys to move the tiles:
```javascript
function handleInput(event) {
  if (isGameOver()) return;

  switch (event.key) {
    case 'ArrowUp':
      moveUp();
      break;
    case 'ArrowDown':
      moveDown();
      break;
    case 'ArrowLeft':
      moveLeft();
      break;
    case 'ArrowRight':
      moveRight();
      break;
    default:
      return;
  }
  addNewTile();
  drawBoard();
}
```
- **Purpose**: Maps user input to the appropriate movement function.
- **Key Concept**: Event handling for keyboard inputs.

### 5. `moveLeft()`, `moveRight()`, `moveUp()`, `moveDown()`
These functions handle tile movement in the respective directions by sliding and combining tiles:
```javascript
function moveLeft() {
  for (let i = 0; i < boardSize; i++) {
    let row = board[i];
    row = slide(row);
    row = combine(row);
    row = slide(row);
    board[i] = row;
  }
}
```
- **Purpose**: Moves tiles in the direction specified.
- **Key Concept**: Uses helper functions `slide()` and `combine()` to achieve the movement logic.

### 6. `slide(row)` and `combine(row)`
- **`slide(row)`**: Moves all non-zero values to the left and fills the rest with zeros.
- **`combine(row)`**: Combines adjacent tiles of the same value, doubling their value and updating the score.

### 7. `isGameOver()`
Checks if there are no possible moves left:
```javascript
function isGameOver() {
  for (let i = 0; i < boardSize; i++) {
    for (let j = 0; j < boardSize; j++) {
      if (board[i][j] === 0) return false;
      if (j < boardSize - 1 && board[i][j] === board[i][j + 1]) return false;
      if (i < boardSize - 1 && board[i][j] === board[i + 1][j]) return false;
    }
  }
  return true;
}
```
- **Purpose**: Determines if the game should end.
- **Key Concept**: Checks for any empty spaces or possible merges.

### 8. `restartGame()`
Resets the game state to start over:
```javascript
function restartGame() {
  document.getElementById('game-over-message').style.display = 'none';
  document.getElementById('restart-button').style.display = 'none';
  initializeBoard();
  drawBoard();
}
```
- **Purpose**: Allows the user to start a new game after losing.
- **Key Concept**: Resets board state and hides the game-over message.

## Learning Objectives
- Understand the basics of **DOM Manipulation** using JavaScript.
- Learn how to implement game logic using **functions** and **arrays**.
- Practice handling **user inputs** and **events**.
- Gain insight into **game state management**.

## License
This project is open source and available under the [MIT License](LICENSE).

## Contributions
Contributions are welcome! Feel free to fork this repository and make your own improvements or bug fixes.

