#include <iostream>
#include <vector>
#include <iomanip>

using namespace std;

// Corrected declaration: 'board' is now a 3x3 array

char board[3][3] = { { '1', '2', '3' }, { '4', '5', '6' }, { '7', '8', '9' } };
char currentPlayer = 'X';
bool gameOver = false;

// Function to display the Tic-Tac-Toe board

void displayBoard() {

    // Optional: Clear the console screen for a cleaner interface (works on Windows/macOS/Linux)
    // system("cls");
    // For some environments like Repl.it, use system("clear");

    cout << "\n\tTic-Tac-Toe\n\n";
    cout << "Player 1 (X)  -  Player 2 (O)" << endl << endl;
    cout << "     |     |     " << endl;
    cout << "  " << board[0][0] << "  |  " << board[0][1] << "  |  " << board[0][2] << endl;
    cout << "_____|_____|_____" << endl;
    cout << "     |     |     " << endl;
    cout << "  " << board[1][0] << "  |  " << board[1][1] << "  |  " << board[1][2] << endl;
    cout << "_____|_____|_____" << endl;
    cout << "     |     |     " << endl;
    cout << "  " << board[2][0] << "  |  " << board[2][1] << "  |  " << board[2][2] << endl;
    cout << "     |     |     " << endl << endl;
}

// Function to get the player's input and update the board

void getPlayerMove() {
    int choice;
    int row, col;
    bool validMove = false;

    while (!validMove) {
        cout << "Player " << currentPlayer << ", enter a number (1-9): ";

        // Handle non-integer input gracefully

        if (!(cin >> choice)) {
            cout << "Invalid input. Please enter a number." << endl;
            cin.clear(); // Clear error flags
            cin.ignore(10000, '\n'); // Discard invalid input
            continue; // Skip the rest of the loop and ask again
        }

        // Convert the user's choice (1-9) to array coordinates (0-2)

        row = (choice - 1) / 3;
        col = (choice - 1) % 3;

        if (choice >= 1 && choice <= 9 && board[row][col] != 'X' && board[row][col] != 'O') {
            board[row][col] = currentPlayer;
            validMove = true;
        } else {
            cout << "Invalid move. That cell is already taken or invalid input. Try again." << endl;
        }
    }
}

// Function to check for a win condition (Logic fixed)

bool checkWin() {

    // Check rows and columns

    for (int i = 0; i < 3; i++) {

        // Check row 'i': all elements must be the same and either 'X' or 'O'

        if (board[i][0] == board[i][1] && board[i][1] == board[i][2]) return true;

        // Check column 'i': all elements must be the same

        if (board[0][i] == board[1][i] && board[1][i] == board[2][i]) return true;
    }

    // Check diagonals (Logic fixed)

    // Top-left to bottom-right

    if (board[0][0] == board[1][1] && board[1][1] == board[2][2]) return true;

    // Top-right to bottom-left

    if (board[0][2] == board[1][1] && board[1][1] == board[2][0]) return true;

    return false;
}

// Function to check for a tie condition checking each array

bool checkTie() {
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (board[i][j] != 'X' && board[i][j] != 'O') {
                return false; // Found an empty cell, not a tie yet
            }
        }
    }
    return true; // All cells are full, and no winner, so it's a tie
}

// Function to switch players

void switchPlayer() {
    if (currentPlayer == 'X') {
        currentPlayer = 'O';
    } else {
        currentPlayer = 'X';
    }
}

int main() {
    while (!gameOver) {
        displayBoard();
        getPlayerMove();

        if (checkWin()) {
            displayBoard();
            cout << "Player " << currentPlayer << " wins! Congratulations!" << endl;
            gameOver = true;
        } else if (checkTie()) {
            displayBoard();
            cout << "It's a tie game!" << endl;
            gameOver = true;
        }

        // Only switch players if the game is still ongoing

        if (!gameOver) {
            switchPlayer();
        }
    }
    // Keeps the console window open until the user presses a key (optional, for some IDEs)

    // cin.ignore();

    // cin.get();

    return 0;
}
