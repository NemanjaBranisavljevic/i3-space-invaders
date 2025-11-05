# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a simple browser-based space shooter game called "EON - pobij sve" (EON - defeat all). It's a classic arcade-style game implemented as a single-page HTML file with embedded CSS and JavaScript.

## Architecture

The game is built as a monolithic HTML file (`index.html`) with all game logic, styling, and rendering contained within it. The architecture follows these key components:

### Game State
- `score`: Current player score (incremented when enemies are destroyed)
- `gameOver`: Boolean flag indicating if the game has ended
- `gameStarted`: Boolean flag tracking if the player has fired the first bullet

### Entity System
The game uses object literals to represent game entities:
- **Player**: Single spaceship at the bottom of the screen with `x`, `y`, `width`, `height`, `speed` properties
- **Enemies**: Grid of 4 rows × 8 columns spawned at the top. Each enemy has position, dimensions, direction, and a sprite image
- **Bullets**: Player's projectiles moving upward with velocity `dy`
- **Enemy Bullets**: Enemies' projectiles moving downward with velocity `dy`

### Game Loop
The game uses `requestAnimationFrame` to maintain smooth 60fps rendering. The loop flow is:
1. Update player position (arrow key input)
2. Update and check collisions for player bullets
3. Update enemy positions and manage enemy direction reversals
4. Generate random enemy shots
5. Update enemy bullets and check collisions with player
6. Render all entities and score

### Enemy Movement
Enemies move horizontally as a group (`enemyDir` controls direction). When any enemy reaches a screen edge, the direction reverses and all enemies shift down by 20 pixels.

### Collision Detection
Uses basic AABB (Axis-Aligned Bounding Box) collision: checks if rectangles overlap on both axes before confirming a hit.

## Running the Game

To run locally:
```bash
# Using http-server (as noted in README.txt)
nohup http-server -p 8080 &
```

Then navigate to: `http://localhost:8080`

Alternatively, simply open `index.html` directly in a browser.

## Game Controls

- **Arrow Left/Right**: Move spaceship left and right
- **Space**: Shoot bullets (first press starts the game)
- **Enter**: Restart after game over (or click the restart button)

## Customization Points

- **Canvas size**: Modify `<canvas width="800" height="600">`
- **Player speed**: Change `player.speed` (default: 5)
- **Enemy grid**: Adjust `rows`, `cols`, `spacingX`, `spacingY` constants
- **Enemy sprite selection**: Currently randomizes from 17 available sprites stored in `/sprites/` folder
- **Bullet speed**: Modify `dy` values in bullet creation (player: -8, enemies: 4)
- **Enemy fire rate**: Adjust probability in `Math.random() < 0.002` check
- **Enemy downward shift**: Modify the `20` pixel value in enemy movement

## Sprite Assets

The game uses 17 PNG sprite files (`sprites/1.png` through `sprites/17.png`) for enemy graphics. These are randomly assigned to each enemy in the grid. Ensure these files exist in the `/sprites/` directory.
