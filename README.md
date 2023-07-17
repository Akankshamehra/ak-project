# tictactoe-cpp-project
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;
class Board {
private:
    char squares[3][3];
public:
    Board() {
        // Initialize the board with numbers 1-9
        int count = 1;
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                squares[i][j] = '0' + count;
                count++;
            }
        }
    }

    void BeginGame() {
        cout << "Welcome to Tic-Tac-Toe!" << endl;
        cout << "Here is the initial board:" << endl;
    }

    void PrintBoard() {
        cout << "-------------" << endl;
        for (int i = 0; i < 3; i++) {
            cout << "| ";
            for (int j = 0; j < 3; j++) {
                cout << squares[i][j] << " | ";
            }
            cout << endl;
            cout << "-------------" << endl;
        }
    }

    bool isValidMove(char num) {
        // Check if the chosen number is valid (between 1-9) and the corresponding square is not already taken
        return (num >= '1' && num <= '9' && squares[(num - '1') / 3][(num - '1') % 3] != 'X' && squares[(num - '1') / 3][(num - '1') % 3] != 'O');
    }

    void playerTurn(char num, char player) {
        // Update the board with the player's move
        squares[(num - '1') / 3][(num - '1') % 3] = player;
    }

    bool CheckWin(char player, bool GameOver) {
        // Check rows
        for (int i = 0; i < 3; i++) {
            if (squares[i][0] == player && squares[i][1] == player && squares[i][2] == player) {
                cout << "Player " << player << " wins!" << endl;
                GameOver = true;
                return GameOver;
            }
        }

        // Check columns
        for (int i = 0; i < 3; i++) {
            if (squares[0][i] == player && squares[1][i] == player && squares[2][i] == player) {
                cout << "Player " << player << " wins!" << endl;
                GameOver = true;
                return GameOver;
            }
        }

        // Check diagonals
        if ((squares[0][0] == player && squares[1][1] == player && squares[2][2] == player) ||
            (squares[0][2] == player && squares[1][1] == player && squares[2][0] == player)) {
            cout << "Player " << player << " wins!" << endl;
            GameOver = true;
            return GameOver;
        }

        return GameOver;
    }

    bool CheckDraw(bool GameOver) {
        int counter = 0;

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                // Check to see if the board is full
                if (squares[i][j] != 'X' && squares[i][j] != 'O') {
                    counter++;
                }
            }
        }
        
        if (counter < 1) {
            cout << "Game Draw\n\n";
            GameOver = true;
        }
        
        return GameOver;
    }
};

int main() {
    srand(time(NULL));
    bool finish = false, GameOver = false;
    char Player = 'O', num;
    int number;

    number = rand() % 2;
    
    if (number == 0) {
        Player = 'X';
    } else {
        Player = 'O';
    }
    
    Board TicTac;
    TicTac.BeginGame();
    TicTac.PrintBoard();
    
    do {
        if (Player == 'X') {
            Player = 'O';
            cout << "Computer's turn: ";
            int number = rand() % 9 + 1;
            char c = '0' + number;
            cout << number << endl;
            if (TicTac.isValidMove(c)) {
                TicTac.playerTurn(c, Player);
            } else {
                continue;
            }
        } else {
            Player = 'X';
            cout << "Player's turn: ";
            cin >> num;
            cout << "\n";
            if (!TicTac.isValidMove(num)) {
                cout << "Invalid move! Please choose a valid number." << endl;
                continue;
            } else {
                TicTac.playerTurn(num, Player);
            }
        }

        TicTac.PrintBoard();
        GameOver = TicTac.CheckWin(Player, GameOver);
        GameOver = TicTac.CheckDraw(GameOver);
        if (GameOver) {
            cout << "Thank you for playing!" << endl;
            finish = true;
        }
    } while (!finish);

    return 0;
}
