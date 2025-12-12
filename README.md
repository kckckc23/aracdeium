# RETRO ARCADE

A collection of classic arcade games built with HTML5, CSS3, and JavaScript featuring neon-styled graphics and authentic sound effects.

## Games Included

### Snake
- Classic snake gameplay with growing mechanics
- Arrow keys to move, Space to pause
- Animated food and smooth snake graphics
- Speed increases as you grow
- **Audio:** Food eating sounds, movement loop, game over sound

**File:** `snakes.html`

### Pong
- Original arcade tennis game
- Mouse controls your paddle
- Ball speed increases with each bounce
- First to 10 points wins
- **Audio:** Ball hit sound on paddle collision

**File:** `pong.html`

### Tic-Tac-Toe
- Strategic 3-in-a-row game
- Click to place X or O
- Glowing neon game board
- Instant replay functionality

**File:** `ttt.html`

### Space Invaders
- Defend Earth from alien waves
- Arrow keys to move, Space to shoot
- Progressive difficulty with each wave
- Classic arcade shooting action
- **Audio:** Shooting sounds, invader death sounds, player death sound

**File:** `spaceinvaders.html`

### Tetris
- Falling blocks puzzle game
- Arrow keys to move/rotate pieces
- Retro terminal styling
- Line clearing mechanics
- **Audio:** Looping background music, line clear sound effects

**File:** `tetris.html`

### Pac-Man
- Navigate maze and eat dots
- Arrow keys or WASD to move
- Avoid ghosts with unique AI
- Power pellets for ghost eating
- **Blinky** (Red): Direct chaser, speeds up
- **Pinky** (Pink): Ambushes ahead of Pac-Man
- **Inky** (Cyan): Wildcard using Blinky/Pac-Man positions
- **Clyde** (Orange): Erratic, chases then flees

**File:** `pacman.html`

## Audio Features

All games include authentic arcade sound effects:
- Background music (Tetris)
- Action sounds (shooting, eating, bouncing)
- Game over sounds
- Looping ambient sounds

Audio files should be placed in `music/` folder with naming convention: `gamename_soundtype.mp3`

## Visual Style

- Neon cyan and magenta color scheme
- Retro-futuristic Orbitron font
- Animated backgrounds with stars
- Glowing effects and smooth transitions
- Responsive design for all devices

## How to Run

1. **Clone the repository:**
   ```bash
   git clone [repository-url]
   cd retro-arcade
   ```

2. **Add audio files (optional):**
   - Create a `music/` folder in the root directory
   - Add audio files following the naming convention:
     - `pong_ballhit.mp3`
     - `snake_food.mp3`, `snake_gameover.mp3`, `snake_movement.mp3`
     - `spaceinvaders_shoot.mp3`, `spaceinvaders_invaderkilled.mp3`, `spaceinvaders_playerdeath.mp3`
     - `tetris_backgroundmusic.mp3`, `tetris_lineclear.mp3`

3. **Launch the arcade:**
   - Open `index.html` in any modern web browser
   - Or open individual game files directly

## File Structure

```
├── index.html          # Main arcade menu
├── snakes.html         # Snake game
├── pong.html          # Pong game
├── ttt.html           # Tic-Tac-Toe game
├── spaceinvaders.html # Space Invaders game
├── tetris.html        # Tetris game
├── pacman.html        # Pac-Man game
└── music/             # Audio files folder
    ├── snake_food.mp3
    ├── snake_gameover.mp3
    ├── snake_movement.mp3
    ├── pong_ballhit.mp3
    ├── spaceinvaders_shoot.mp3
    ├── spaceinvaders_invaderkilled.mp3
    ├── spaceinvaders_playerdeath.mp3
    ├── tetris_backgroundmusic.mp3
    └── tetris_lineclear.mp3
```

## Features

### Visual Design
- Neon cyberpunk aesthetic with cyan and magenta color scheme
- Animated starfield background with twinkling effects
- Gradient text effects and glowing borders
- Responsive design for desktop and mobile
- Smooth animations and hover effects

### Audio System
- Modern browser compatibility with autoplay policy handling
- Volume-balanced sound effects for optimal gameplay
- Looping background music where appropriate
- Error handling for missing audio files

### Gameplay Features
- Authentic arcade mechanics faithful to original games
- Progressive difficulty in multiple games
- High score tracking and game state management
- Keyboard controls with arrow key scroll prevention
- Pause/resume functionality where applicable

### Game-Specific Features
- **Snake:** Grid-based movement, speed progression, animated food
- **Pong:** Accelerating ball physics, AI opponent
- **Space Invaders:** Wave-based progression, formation movement
- **Tetris:** Authentic piece rotation, line clearing animations
- **Pac-Man:** Classic ghost AI personalities, power pellet mode
- **Tic-Tac-Toe:** Win detection, replay functionality

## Controls

### Universal
- **Mouse:** Navigate menus and control paddles (Pong)
- **Keyboard shortcuts:** Press 1-6 on main menu to launch games quickly

### Game-Specific
- **Snake:** Arrow keys to move, Space to pause
- **Pong:** Mouse to control paddle
- **Tic-Tac-Toe:** Click cells to place X/O
- **Space Invaders:** Arrow keys to move, Space to shoot
- **Tetris:** Arrow keys to move/rotate, Space for hard drop, P to pause
- **Pac-Man:** Arrow keys or WASD to move

## Technical Details

- Pure HTML5/CSS3/JavaScript - No frameworks or dependencies
- Canvas-based rendering for smooth animations
- RequestAnimationFrame for 60fps gameplay
- Modern ES6+ JavaScript with proper error handling
- CSS Grid and Flexbox for responsive layouts
- Web Audio API integration with fallback handling

## Browser Compatibility

- **Chrome/Chromium** (Recommended)
- **Firefox**
- **Safari**
- **Edge**
- **Internet Explorer** (Limited support)

## Future Enhancements

- Additional classic arcade games
- Online leaderboards
- Customizable controls
- Game difficulty settings
- More audio themes
- Mobile touch controls optimization

**Built with care for retro gaming enthusiasts**