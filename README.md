# 8-Queen-Hill-Climbing

Hill Climbing adalah salah satu metode yang digunakan untuk menyelesaikan permasalahan pencarian dengan jarak terdekat. 
Cara kerjanya adalah menentukan langkah berikutnya dengan menempatkan node yang akan muncul sedekat mungkin dengan sasarannya. 
Untuk proses pengujiannya digunakan fungsi Heuristik. s  yang berupa    fungsi    heuristik    ini    akan    menunjukkan seberapa baiknya nilai terkaan yang diambil terhadap keadaan keadaan lainnya yang mungkin.
Algoritma ini berisi memori yang  efisien  karena  tidak  mempertahankan  pohon pencaharian namun hanya dapat terlihat pada kondisisaat ini, dan state yg akan datang. 

Untuk permasalahan yang akan dijelaskan adalah 8-queen.
Pembahasan pada program permasalahan 8-queen dengan menggunakan Hill Climbing sebagai berikut :
1. Fungsi untuk menghitung maximal local
```
int calcEvaluationFunction(int (&board)[N])
{
	int cnt = 0;
	for(int i = 0; i < 8; ++i)
	{
		for(int j = i+1; j < 8; ++j)
		{
			if(board[i] == board[j])
				++cnt;
			else if(abs(i-j) == abs(board[i]-board[j]))
				++cnt;
		}
	}
	return 28-cnt;
}
```
2. Fungsi untuk print board.
```
void printBoard(int (&board)[N])
{
	int grid[10][10];
	memset(grid, 0, sizeof grid);
	for(int i = 0; i < 8; ++i)
		grid[ board[i] ][i] = 1;
	for(int i = 0; i < 8; ++i)
	{
		for(int j = 0; j < 8; ++j)
			cout << grid[i][j] << " ";
		cout << endl;
	}
	return;
}
```
3. Fungsi untuk meng-copy board yang telah dibuat ke board lainnya
```
void copyBoard(int (&board1)[N], int (&board2)[N])
{
	for(int i = 0; i < 8; ++i)
		board1[i] = board2[i];
}
```
4. Fungsi dalam pencarian secara Hill Climbing
```
bool hillClimbing(int (&board)[N])
{
	int bestBoard[N], bestValue, boardValue;
	copyBoard(bestBoard, board);

	int cnt = 0;
	while(1)
	{
		int currBoard[N], currValue;
		copyBoard(currBoard, board);
		boardValue = bestValue = calcEvaluationFunction(board);

		if(bestValue == 28)
		{
			cout << "Reached Global Maxima after " << cnt << " moves" << endl;
			printBoard(bestBoard);
			return 1;
		}

		++cnt;
		for(int i = 0; i < 8; ++i)
		{
			int temp = currBoard[i];
			for(int j = 0; j < 8; ++j)
			{
				if(j == board[i])
					continue;
				currBoard[i] = j;
				currValue = calcEvaluationFunction(currBoard);
				if(currValue > bestValue)
				{
					bestValue = currValue;
					copyBoard(bestBoard, currBoard);
				}
			}
			currBoard[i] = temp;
		}
		if(bestValue <= boardValue)
		{
			cout << "Stucked in Local Maxima after " << cnt << " moves" << endl;
			printBoard(bestBoard);
			return 0;
		}
		copyBoard(board, bestBoard);
	}
}
```
