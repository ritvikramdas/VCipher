# VCipher
Implementation of a Common Cipher

#include <iostream>
#include <fstream>
#include <stdio.h>
#include <stdlib.h>
#include <cstdlib>
#include <ctime>

using namespace std;

void clearBuff ( char[] , long );

int main() {

	int choice = -1;

	cout << "Welcome to the Vigenere cipher!" << endl;
	cout << "Enter 0 for encryption or 1 for decryption." << endl;
	cin >> choice;

	while ( choice != 0 && choice != 1 )
	{
		cout << "Incorrect input. Please enter 0 for encryption or 1 for decryption." << endl;
		cin >> choice;
	}

	long BUFFERSIZE = 4000;

	FILE * inFile;
	FILE * oFile;

	char* theFile = new char[128];

	char* outFile = new char[128];

	cout << "What is the name of the input file?" << endl;

	cin >> theFile;

	cout << "What is the name of the output file?" << endl;

	cin >> outFile;

	inFile = fopen( theFile, "rb");

	char inputKey[128];

	int counter = 0;

	cout << "Enter the key." << endl;

	cin >> inputKey;

	for ( int i = 0; i < BUFFERSIZE; i++ )
	{
		if ( inputKey[i] == '\0' )
		{
			break;
		}

		else
		{
			counter++;
		}
	}

	char key[counter];

	for ( int k = 0; k < counter; k++ )
	{
		key[k] = inputKey[k];
	}

	if ( choice == 0 )
	{
		oFile = fopen(outFile,"wb");

		int reads = 0;

		do
		{
			int BUFFSIZE = 4000;

			char bufferIn[BUFFSIZE];

			reads = fread(bufferIn, sizeof(char), BUFFSIZE, inFile);

			int realCount = 0;

			for ( int i = 0; bufferIn[i] != '\0' && i < BUFFSIZE; i++ )
			{
				realCount++;
			}

			char bufferOut[realCount];

			char keyToTable[realCount];
			int k = 0;

			for (int i = 0; i < realCount-1; i++)
			{
				if ( k < counter )
				{
					keyToTable[i] = key[k];
				}

				else
				{
					k = 0;
					keyToTable[i] = key[k];
				}
				k++;
			}

			int vigTable[realCount-1][117];

			for ( int u=0; u < realCount-1; u++)
			{
				vigTable[u][0] = keyToTable[u];
			}

			for ( int m = 1; m<117; m++ )
			{
				for (int u = 0; u < realCount-1; u++)
				{
					if (((vigTable[u][m-1])+1) > 126)
					{
						vigTable[u][m] = ((vigTable[u][m-1])+1) - 117;
					}
					else
					{
						vigTable[u][m] = (vigTable[u][m-1])+1;
					}
				}
			}

			int m = 0;

			for ( int f = 0; f < 117; f++ )
			{
				for ( int i = 0; i < realCount - 1; i++ )
				{
					m = f + 10;

					if ( ((int)bufferIn[i]) == m)
					{
						bufferOut[i] = ((char) vigTable[i][f]);
					}
				}
			}

			fwrite(bufferOut, sizeof(char), sizeof(bufferOut), oFile);

			clearBuff(bufferIn, BUFFSIZE);

		}while (reads == BUFFERSIZE);

	}

	if ( choice == 1 ) {

		oFile = fopen(outFile,"wb");

		int reads = 0;

		do
		{
			int BUFFSIZE = 4000;

			char bufferIn[BUFFSIZE];

			reads = fread(bufferIn, sizeof(char), BUFFSIZE, inFile);

			int realCount = 0;

			for ( int i = 0; bufferIn[i] != '\0' && i < BUFFSIZE; i++ )
			{
				realCount++;
			}

			char bufferOut[realCount];

			char keyToTable[realCount];
			int k = 0;

			for (int i = 0; i < realCount-1; i++)
			{
				if ( k < counter )
				{
					keyToTable[i] = key[k];
				}

				else
				{
					k = 0;
					keyToTable[i] = key[k];
				}
				k++;
			}

			int vigTable[realCount-1][117];

			for ( int u=0; u < realCount-1; u++)
			{
				vigTable[u][0] = keyToTable[u];
			}

			for ( int m = 1; m<117; m++ )
			{
				for (int u = 0; u < realCount-1; u++)
				{
					if (((vigTable[u][m-1])+1) > 126)
					{
						vigTable[u][m] = ((vigTable[u][m-1])+1) - 117;
					}
					else
					{
						vigTable[u][m] = (vigTable[u][m-1])+1;
					}
				}
			}

			int m = 0;

				for ( int f = 0; f < 117; f++ )
				{
					for ( int i = 0; i < realCount - 1; i++ )
					{
						m = f + 10;

						if ( ((int)bufferIn[i]) == vigTable[i][f])
						{
							bufferOut[i] = ((char)m);
						}
					}
				}

				fwrite(bufferOut, sizeof(char), sizeof(bufferOut), oFile);

				clearBuff(bufferIn, BUFFSIZE);

		} while ( reads == BUFFERSIZE );

	}



	return 0;

}

void clearBuff ( char filledArray[] , long BUFFERSIZE )
{
	long i;

	for ( i = 0; i < BUFFERSIZE; i++ )
	{
		filledArray[i] = '\0';
	}
}
