#include <stdio.h>
#include <stdlib.h>

#define SIZE 3

char board[SIZE][SIZE];

void initializeBoard() {
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            board[i][j] = '1' + (i * SIZE + j);
        }
    }
}

void displayBoard() {
    printf("\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf(" %c ", board[i][j]);
            if (j < SIZE - 1) printf("|");
        }
        printf("\n");
        if (i < SIZE - 1) printf("---|---|---\n");
    }
    printf("\n");
}

int isWinner() {
    for (int i = 0; i < SIZE; i++) {
        // Check rows and columns
        if ((board[i][0] == board[i][1] && board[i][1] == board[i][2]) ||
            (board[0][i] == board[1][i] && board[1][i] == board[2][i])) {
            return 1;
        }
    }
    // Check diagonals
    if ((board[0][0] == board[1][1] && board[1][1] == board[2][2]) ||
        (board[0][2] == board[1][1] && board[1][1] == board[2][0])) {
        return 1;
    }
    return 0;
}

int isDraw() {
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (board[i][j] != 'X' && board[i][j] != 'O') {
                return 0;
            }
        }
    }
    return 1;
}

void playGame() {
    int player = 1;
    char mark;
    int choice;
    int row, col;

    while (1) {
        displayBoard();
        player = (player % 2) ? 1 : 2;
        mark = (player == 1) ? 'X' : 'O';

        printf("Player %d, enter a number to place your mark (%c): ", player, mark);
        scanf("%d", &choice);

        if (choice < 1 || choice > 9) {
            printf("Invalid choice! Try again.\n");
            continue;
        }

        row = (choice - 1) / SIZE;
        col = (choice - 1) % SIZE;

        if (board[row][col] == 'X' || board[row][col] == 'O') {
            printf("Cell already taken! Try again.\n");
            continue;
        }

        board[row][col] = mark;

        if (isWinner()) {
            displayBoard();
            printf("Player %d wins!\n", player);
            break;
        }

        if (isDraw()) {
            displayBoard();
            printf("It's a draw!\n");
            break;
        }

        player++;
    }
}

int main() {
    printf("Welcome to Tic Tac Toe!\n");
    initializeBoard();
    playGame();
    return 0;
}
