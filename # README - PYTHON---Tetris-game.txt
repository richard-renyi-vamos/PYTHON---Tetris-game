CLAUDE CONVERSATION

-------------

I can help you create a basic Tetris game in Python using the Pygame library. Here's a simplified version to get you started:

```python
import pygame
import random

# Initialize Pygame
pygame.init()

# Colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
CYAN = (0, 255, 255)
YELLOW = (255, 255, 0)
MAGENTA = (255, 0, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
ORANGE = (255, 165, 0)

# Game dimensions
BLOCK_SIZE = 30
GRID_WIDTH = 10
GRID_HEIGHT = 20
SCREEN_WIDTH = BLOCK_SIZE * GRID_WIDTH
SCREEN_HEIGHT = BLOCK_SIZE * GRID_HEIGHT

# Tetromino shapes
SHAPES = [
    [[1, 1, 1, 1]],
    [[1, 1], [1, 1]],
    [[1, 1, 1], [0, 1, 0]],
    [[1, 1, 1], [1, 0, 0]],
    [[1, 1, 1], [0, 0, 1]],
    [[1, 1, 0], [0, 1, 1]],
    [[0, 1, 1], [1, 1, 0]]
]

COLORS = [CYAN, YELLOW, MAGENTA, RED, GREEN, BLUE, ORANGE]

class Tetromino:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.shape = random.choice(SHAPES)
        self.color = random.choice(COLORS)

    def move(self, dx, dy):
        self.x += dx
        self.y += dy

    def rotate(self):
        self.shape = list(zip(*self.shape[::-1]))

class TetrisGame:
    def __init__(self):
        self.screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Tetris")
        self.clock = pygame.time.Clock()
        self.grid = [[BLACK for _ in range(GRID_WIDTH)] for _ in range(GRID_HEIGHT)]
        self.current_piece = self.new_piece()
        self.game_over = False

    def new_piece(self):
        return Tetromino(GRID_WIDTH // 2 - 1, 0)

    def draw_grid(self):
        for y, row in enumerate(self.grid):
            for x, color in enumerate(row):
                pygame.draw.rect(self.screen, color, (x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE), 0)

    def draw_piece(self):
        for y, row in enumerate(self.current_piece.shape):
            for x, cell in enumerate(row):
                if cell:
                    pygame.draw.rect(self.screen, self.current_piece.color,
                                     ((self.current_piece.x + x) * BLOCK_SIZE,
                                      (self.current_piece.y + y) * BLOCK_SIZE,
                                      BLOCK_SIZE, BLOCK_SIZE), 0)

    def check_collision(self, dx=0, dy=0):
        for y, row in enumerate(self.current_piece.shape):
            for x, cell in enumerate(row):
                if cell:
                    new_x = self.current_piece.x + x + dx
                    new_y = self.current_piece.y + y + dy
                    if (new_x < 0 or new_x >= GRID_WIDTH or
                        new_y >= GRID_HEIGHT or
                        (new_y >= 0 and self.grid[new_y][new_x] != BLACK)):
                        return True
        return False

    def merge_piece(self):
        for y, row in enumerate(self.current_piece.shape):
            for x, cell in enumerate(row):
                if cell:
                    self.grid[self.current_piece.y + y][self.current_piece.x + x] = self.current_piece.color

    def remove_full_rows(self):
        full_rows = [i for i, row in enumerate(self.grid) if all(cell != BLACK for cell in row)]
        for row in full_rows:
            del self.grid[row]
            self.grid.insert(0, [BLACK for _ in range(GRID_WIDTH)])

    def run(self):
        while not self.game_over:
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    self.game_over = True
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_LEFT and not self.check_collision(-1):
                        self.current_piece.move(-1, 0)
                    if event.key == pygame.K_RIGHT and not self.check_collision(1):
                        self.current_piece.move(1, 0)
                    if event.key == pygame.K_DOWN and not self.check_collision(0, 1):
                        self.current_piece.move(0, 1)
                    if event.key == pygame.K_UP:
                        self.current_piece.rotate()
                        if self.check_collision():
                            self.current_piece.rotate()
                            self.current_piece.rotate()
                            self.current_piece.rotate()

            if not self.check_collision(0, 1):
                self.current_piece.move(0, 1)
            else:
                self.merge_piece()
                self.remove_full_rows()
                self.current_piece = self.new_piece()
                if self.check_collision():
                    self.game_over = True

            self.screen.fill(BLACK)
            self.draw_grid()
            self.draw_piece()
            pygame.display.flip()
            self.clock.tick(5)

        pygame.quit()

if __name__ == "__main__":
    game = TetrisGame()
    game.run()
```

This code provides a basic implementation of Tetris. Here's a breakdown of its main components:

1. We import the necessary libraries: Pygame for the game framework and random for generating random pieces.

2. We define constants for colors, game dimensions, and Tetromino shapes.

3. The `Tetromino` class represents a single Tetris piece, with methods to move and rotate.

4. The `TetrisGame` class handles the main game logic, including:
   - Initializing the game
   - Creating new pieces
   - Drawing the grid and pieces
   - Checking for collisions
   - Merging pieces into the grid
   - Removing full rows
   - Handling user input
   - Running the main game loop

5. In the main game loop, we:
   - Handle events (quitting and key presses)
   - Move the current piece down
   - Check for collisions and merge the piece if necessary
   - Create a new piece if the current one has been merged
   - Draw the updated game state
   - Update the display

This implementation includes basic Tetris functionality, but there's room for improvement and additional features. Would you like me to explain any part of the code in more detail or suggest some enhancements?
