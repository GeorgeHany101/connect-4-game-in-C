#include <stdio.h>

#define BOARD_ROW 8
#define BOARD_COLUMN 8

int board[BOARD_ROW][BOARD_COLUMN];
int board_ref[BOARD_COLUMN];

void Game();
void Game_Run();
void layout();
int Check_Winner(int symbol);

int main(){
	Game();
	return 0;
}

void Game()
{
	char selection;

	printf("\"WELCOME TO CONNECT FOUR\"\n\n\t~To play press any key\n\t~To exit press (E)\n");

	scanf("%c", &selection);

	if (selection == 'e' || selection == 'E')
		selection = 0;

	if (selection)
		Game_Run();

	printf("\n\nThank You For Playing\nDone by: George Hany\nAbdelrahman Ayman\nHamza Haitham");
}

void Game_Run()
{
	int choice;
	int symbol = 1;
	int is_winner = 0;

	for (int i = 0; i < 64; ++i) {
		layout();
		printf("choose a column:");
		scanf("%d", &choice);
		while (choice < 1 || choice > BOARD_COLUMN || board_ref[choice - 1] >= BOARD_ROW) { 
			printf("Invalid Choice!..Try Again..\n");
			scanf("%d", &choice);
		}
		board[(BOARD_ROW - 1) - board_ref[choice - 1]][choice - 1] = symbol;
		board_ref[choice - 1]++;

		if (Check_Winner(symbol)) {
			is_winner = 1;
			break;
		}

		if (symbol == 1)
			symbol = 2;
		else
			symbol = 1;
	}

	if (is_winner) 
		printf("Player %d is the winner\n", symbol);
	else
		printf("TIE!\n");
}

int Check_Winner(int symbol){
	int count, checker;

//horizontal
	checker = BOARD_COLUMN - 3;
	for (int i = 0; i < BOARD_ROW; ++i) {
		for (int j = 0; j <= checker; ++j) {
			count = 0;
			for (int k = 0; k < 4; ++k) {
				if (board[i][j + k] == symbol)
					count++;
			}

			if (count == 4)
				return 1;
		}
	}

//vertical
	checker = BOARD_ROW - 3;
	for (int j = 0; j < BOARD_COLUMN; ++j) {
		for (int i = 0; i <= checker; ++i) {
			count = 0;
			for (int k = 0; k < 4; ++k) {
				if (board[i + k][j] == symbol)
					count++;
			}

			if (count == 4)
				return 1;
		}
	}

//diagonals left to right
	int i2,j2;
	for(int i=1;i<BOARD_ROW-1;i++){
		for(int j=1;j<BOARD_COLUMN-1;j++){
			count=0;
			for(i2=i,j2=j;(i2>=0)||(j2>=0);i2--,j2--){
				if (board[i2][j2] == symbol){
                    count++;
                    if (count == 4) 
					return 1;    
                }
                else
                    break;
			}
			for (i2 = i+1, j2 = j+1; (i2 <= BOARD_COLUMN-1) || (j2 <= BOARD_COLUMN-1); i2++, j2++){
                if (board[i2][j2] == symbol){
                    count++;
                    if (count == 4) 
					return 1;
                }
                else
                    break;
            }

//diagonals right to left
			count=0;
			for (i2 = i, j2 = j; (i2 <= BOARD_ROW-1) || (j2 >= 0); i2++, j2--)
            {
                if (board[i2][j2] == symbol){
                    count++;
                    if (count == 4) 
					return 1;    
                }
                else
                    break;
            }
			for (i2 = i-1, j2 = j+1; (i2 >= 0) || (j2 <= BOARD_COLUMN-1); i2--, j++){
                if (board[i2][j2] == symbol){
                    count++;
                    if (count == 4) 
					return 1;
                }
                else
                    break;
            }
		}
	}

	return 0;
}


void layout(){
	char divider[] = "|___|___|___|___|___|___|___|___|";

	printf("  1   2   3   4   5   6   7   8\n");
	printf("%s\n", divider);

	for (int i = 0; i < 8; i++) {
		for (int j = 0; j < 8; j++) {
			printf("| %d ", board[i][j]);
		}

		printf("|\n");
		printf("%s\n", divider);
	}
}
