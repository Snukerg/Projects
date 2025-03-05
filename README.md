import pygame
import random

WIDTH, HEIGHT = 600, 600
GRID_SIZE = 10
CELL_SIZE = WIDTH // GRID_SIZE
FONT_SIZE = 40

words = ["КОТ", "СОБАКА", "ЯБЛОКО", "СЕНАТ",
              "РОБОТ", "ГОРИЗОНТ", "ДЖАЗ", "ТЫКВА",
              "ОСТРОВ", "КИНЕТИКА", "ЗВУК", "ЗРЕНИЕ",
              "СЕРДЦЕ", "ЖЕЛЕЗО",
             "СЛОВО", "ДРУГ", "КОШКА", "СЕМЬЯ", "ПЛАТИНА", "ВОЛЬФРАМ", "ЛАТУНЬ", "ШЕНОК", "ЛОДКА", "АНДРОЙД", "КИБОРГ", 
             "РЫБА", "КОРАБЛЬ", "ПАЛАДИЙ"]

WORDS = random.sample(words, 7)
def create_word_search(words, grid_size):
    grid = [[' ' for _ in range(grid_size)] for _ in range(grid_size)]

    for word in words:
        placed = False
        while not placed:
            direction = random.choice(['horizontal', 'vertical'])
            if direction == 'horizontal':
                row = random.randint(0, grid_size - 1)
                col = random.randint(0, grid_size - len(word))
            else:  
                row = random.randint(0, grid_size - len(word))
                col = random.randint(0, grid_size - 1)

            if can_place(grid, word, row, col, direction):
                place_word(grid, word, row, col, direction)
                placed = True

    fill_empty_cells(grid)
    return grid


def can_place(grid, word, row, col, direction):
    for i in range(len(word)):
        if direction == 'horizontal':
            if grid[row][col + i] not in (' ', word[i]):
                return False
        else:  
            if grid[row + i][col] not in (' ', word[i]):
                return False
    return True


def place_word(grid, word, row, col, direction):
    for i in range(len(word)):
        if direction == 'horizontal':
            grid[row][col + i] = word[i]
        else:  
            grid[row + i][col] = word[i]


def fill_empty_cells(grid):
    for i in range(len(grid)):
        for j in range(len(grid[i])):
            if grid[i][j] == ' ':
                grid[i][j] = random.choice('АБВГДЕЁЖЗИЙКЛМНОПРСТУФХЦЧШЩЪЫЬЭЮЯ')


def draw_grid(screen, grid):
    font = pygame.font.Font(None, FONT_SIZE)
    for i in range(GRID_SIZE):
        for j in range(GRID_SIZE):
            cell_value = grid[i][j]
            text = font.render(cell_value, True, (0, 0, 0))
            screen.blit(text, (j * CELL_SIZE + (CELL_SIZE - text.get_width()) // 2,
                               i * CELL_SIZE + (CELL_SIZE - text.get_height()) // 2))
            pygame.draw.rect(screen, (200, 200, 200), (j * CELL_SIZE, i * CELL_SIZE, CELL_SIZE, CELL_SIZE), 1)


def main():
    pygame.init()
    screen = pygame.display.set_mode((WIDTH, HEIGHT))
    pygame.display.set_caption("Филворд")

    word_search_grid = create_word_search(WORDS, GRID_SIZE)

    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        screen.fill((255, 255, 255))
        draw_grid(screen, word_search_grid)
        pygame.display.flip()

    pygame.quit()


if __name__ == "__main__":
    main()
