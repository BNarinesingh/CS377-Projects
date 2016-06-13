//N Queen Project.................................................................................................
//CS377 N Queen Puzzle Project Copyright (c) 2016 Aleem Anula, Barindra Narinesingh and Willy William. All rights reserved.
//Code works in Dev-C++ 5.11 and XCode and Visual Studios Express 2015
//Code uses dynamic array as our board and allowed us to change the number of queens based on user input
//Reference CUNY YORK CS 377 slides, https://en.wikipedia.org/wiki/Eight_queens_puzzle for how to solve the puzzle
//and https://en.wikibooks.org/wiki/Algorithm_Implementation/Miscellaneous/N-Queens for outline and backtracking
//Our code used the Algorithm Implementation and built upon it to include all cases of N
//we originally designed our code like the class notes however found out that the code could not have the user alter
//the number of queens while the code was running. It had to be changed in the code then compiled again.

/*
Size	Nodes_Exp	Num_Solutions 	Est_Time
1		1			1				0.006
2		2			0				0.003
3		5			0				0.004
4		16			2				0.03
5		53			10				0.762
6		152			4				0.266
7		551			40				4.463
8		2056		92				11.403
9		8393		352				53.243
10		35538		724				103.074
*/

#include <iostream>
#include <cstdlib>
#include <ctime>

int size = 9;        // Temporary size declared so we can use the array.

int *position = new int[size];    // Dynamic array allowed us to generate any size board
int solutions = 0;
int nodes_expanded = 0;


bool check(int i, int Qi)			// Checks to make sure the queens don't overlap
{
	for (int j = 0; j < i; j++)
	{
		int Qj = position[j];

		if (Qj == Qi)				// Qi, Qj can not be in the same column/diagonal
			return false;
		else if (Qj == Qi - (i - j))  
			return false;
		else if (Qj == Qi + (i - j))  
			return false;
		else
			continue;
	}

	return true;
}

void printSolution(int k) {			// Prints Solution

	std::cout << "Solution: ";

	for (int i = 0; i < size; i++)
		std::cout << position[i]+1 << " ";		// added 1 to start at 1 instead of 0

	std::cout << "\n";
	solutions++;

	std::cout << "\n";

	for (int a = 1; a <= size; a++)
		std::cout << "....";
	
	std::cout << "\n";

	for (int x = 0; x<size; x++)
	{
		std::cout << ": ";

		for (int y = 0; y<size; y++)
		{
			if (x == position[y])
				std::cout << "" << "Q : ";
			else
				std::cout << " " << " : ";
		}

		std::cout << "\n";
		for (int a = 1; a <= size; a++)
			std::cout << "....";
		std::cout << "\n";
	}
	std::cout << "\n" << "-------------------------\n";



}


void Backtrack(int k)	// If k is a solution report it, else generate boards
{
	if (k == size) 
	{
		printSolution(k);
	}

	else
	{
		for (int i = 0; i < size; i++) // Generates the boards 
		{
			if (check(k, i))		// Tests the boards
			{
				position[k] = i;		
				Backtrack(k + 1);		//Recursively Backtrack and place another queen
				nodes_expanded++;
			}
			else
				continue;
		}
	}
}

int main()
{
	clock_t start_t;

	int q;
	int k = 0;

	std::cout << "--------------\n";
	std::cout << "Welcome to the N Queen solver!\n";
	std::cout << "Enter amount of Queens to consider \n";
	std::cin >> q;

	
	do {
		if (q == 2 || q == 3) {
			std::cout << "It is impossible to place this many queens on a " << q << " x " << q << " board \n";
			std::cout << "Please try another amount of queens to consider: \n";
			std::cin >> q;
		}
	} while (q == 2 || q == 3); // case 2 and 3 are impossible so we removed them
	
	

	std::cout << "\n";
	size = q;
	start_t = clock();

	Backtrack(k);

	std::cout << "Number of solutions: " << solutions << "\n";
	std::cout << "Number of nodes explored: " << nodes_expanded << "\n";
	std::cout << "\nTime taken for process to complete: " << ((float)(clock() - start_t) / (float)CLOCKS_PER_SEC) << " seconds. \n\n\n";


	system("pause");
	delete[] position;		// Restores the memory allocated by the dynamic array

	return 0;
}


