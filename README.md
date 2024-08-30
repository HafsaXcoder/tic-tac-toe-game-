import pygame
import sys

# Initialize Pygame
pygame.init()

# Constants
SCREEN_SIZE = 300
LINE_WIDTH = 10
CELL_SIZE = SCREEN_SIZE // 3
WHITE = (255, 255, 255)
LINE_COLOR = (0, 0, 0)
PLAYER_X_COLOR = (255, 0, 0)
PLAYER_O_COLOR = (0, 0, 255)
FONT = pygame.font.Font(None, 48)

# Initialize screen
screen = pygame.display.set_mode((SCREEN_SIZE, SCREEN_SIZE))
pygame.display.set_caption("Tic Tac Toe")

# Board variables
board = [[' ' for _ in range(3)] for _ in range(3)]
current_player = 'X'
game_over = False

def draw_board():
    for i in range(1, 3):
        pygame.draw.line(screen, LINE_COLOR, (i * CELL_SIZE, 0), (i * CELL_SIZE, SCREEN_SIZE), LINE_WIDTH)
        pygame.draw.line(screen, LINE_COLOR, (0, i * CELL_SIZE), (SCREEN_SIZE, i * CELL_SIZE), LINE_WIDTH)

def draw_symbols():
    for row in range(3):
        for col in range(3):
            symbol = board[row][col]
            if symbol == 'X':
                pygame.draw.line(screen, PLAYER_X_COLOR, (col * CELL_SIZE + 20, row * CELL_SIZE + 20),
                                 ((col + 1) * CELL_SIZE - 20, (row + 1) * CELL_SIZE - 20), 5)
                pygame.draw.line(screen, PLAYER_X_COLOR, ((col + 1) * CELL_SIZE - 20, row * CELL_SIZE + 20),
                                 (col * CELL_SIZE + 20, (row + 1) * CELL_SIZE - 20), 5)
            elif symbol == 'O':
                pygame.draw.circle(screen, PLAYER_O_COLOR,
                                   (int(col * CELL_SIZE + CELL_SIZE / 2), int(row * CELL_SIZE + CELL_SIZE / 2)),
                                   int(CELL_SIZE / 2) - 20, 5)

def check_winner():
    # Check rows
    for row in range(3):
        if board[row][0] == board[row][1] == board[row][2] != ' ':
            return board[row][0]

    # Check columns
    for col in range(3):
        if board[0][col] == board[1][col] == board[2][col] != ' ':
            return board[0][col]

    # Check diagonals
    if board[0][0] == board[1][1] == board[2][2] != ' ':
        return board[0][0]
    if board[0][2] == board[1][1] == board[2][0] != ' ':
        return board[0][2]

    # Check for tie
    if all(symbol != ' ' for row in board for symbol in row):
        return 'Tie'

    return None

def draw_winner(winner):
    winner_text = FONT.render(f"{winner} wins!", True, (0, 0, 0))  # Black color for winner message
    screen.blit(winner_text, (SCREEN_SIZE // 2 - winner_text.get_width() // 2, SCREEN_SIZE // 2 - winner_text.get_height() // 2))
    pygame.display.flip()

def draw_tie():
    tie_text = FONT.render("It's a tie!", True, (0, 0, 0))  # Black color for tie message
    screen.blit(tie_text, (SCREEN_SIZE // 2 - tie_text.get_width() // 2, SCREEN_SIZE // 2 - tie_text.get_height() // 2))
    pygame.display.flip()

def restart_game():
    global board, current_player, game_over
    board = [[' ' for _ in range(3)] for _ in range(3)]
    current_player = 'X'
    game_over = False

def main():
    global current_player, game_over
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            elif event.type == pygame.MOUSEBUTTONDOWN and not game_over:
                if event.button == 1:
                    pos = pygame.mouse.get_pos()
                    row, col = pos[1] // CELL_SIZE, pos[0] // CELL_SIZE
                    if board[row][col] == ' ':
                        board[row][col] = current_player
                        current_player = 'O' if current_player == 'X' else 'X'

            elif event.type == pygame.KEYDOWN and not game_over:
                if event.key == pygame.K_SPACE:
                    row, col = pygame.mouse.get_pos()[1] // CELL_SIZE, pygame.mouse.get_pos()[0] // CELL_SIZE
                    if board[row][col] == ' ':
                        board[row][col] = current_player
                        current_player = 'O' if current_player == 'X' else 'X'

        screen.fill(WHITE)
        draw_board()
        draw_symbols()

        winner = check_winner()
        if winner:
            game_over = True
            if winner != 'Tie':
                draw_winner(winner)
            else:
                draw_tie()
            pygame.time.wait(2000)
            restart_game()

        pygame.display.flip()

if __name__ == "__main__":
    main()






