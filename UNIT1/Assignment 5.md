# Assignment 5

## Title : 
Develop a program to compute the fast transpose of a sparse matrix using its compact (triplet) representation efficiently.

## Theory: 
A sparse matrix is a matrix in which most elements are zero. Storing only non-zero elements reduces memory usage. The triplet (compact) representation stores a list of triples (row, col, value) for each non-zero element.

Fast transpose of a sparse matrix produces the compact representation of the transposed matrix efficiently (in O(nz + cols) time, where nz = number of non-zero elements). The idea:

Count how many non-zero elements are in each column of the original matrix (these become rows in the transpose).

Compute the starting index (position) in the transposed array for each column using prefix sums of these counts.

Place each element from the original triplet list into its correct position in the transposed triplet array by using the starting index and incrementing it.

This avoids repeated scanning or insertion and is cache-friendly for the compact arrays.

## Code
```cpp

#include <iostream>
using namespace std;

#define MAX 1000 

struct SparseMatrix
{
    int row;
    int col;
    int value;
};

int main()
{
    SparseMatrix mat[MAX], fastTrans[MAX];
    int m, n, numTerms;

    cout << "Enter the number of rows and columns of the matrix: ";
    cin >> m >> n;

    cout << "Enter the number of non-zero elements: ";
    cin >> numTerms;

    
    if (numTerms > MAX - 1)
    {
        cout << "Too many non-zero elements!\n";
        return 0;
    }

    
    cout << "Enter row, column and value for each non-zero element:\n";
    for (int i = 1; i <= numTerms; i++)
    {
        cin >> mat[i].row >> mat[i].col >> mat[i].value;

        
        if (mat[i].row >= m || mat[i].col >= n || mat[i].row < 0 || mat[i].col < 0)
        {
            cout << "Invalid row or column index entered!\n";
            return 0;
        }
    }

    
    mat[0].row = m;
    mat[0].col = n;
    mat[0].value = numTerms;

    
    fastTrans[0].row = mat[0].col;
    fastTrans[0].col = mat[0].row;
    fastTrans[0].value = mat[0].value;

    if (numTerms > 0)
    {
        int row_terms[MAX] = {0}; 
        int start_pos[MAX] = {0}; 

        
        for (int i = 1; i <= numTerms; i++)
            row_terms[mat[i].col]++;

        
        start_pos[0] = 1;
        for (int i = 1; i < n; i++)
            start_pos[i] = start_pos[i - 1] + row_terms[i - 1];

        
        for (int i = 1; i <= numTerms; i++)
        {
            int j = start_pos[mat[i].col]++;
            fastTrans[j].row = mat[i].col;
            fastTrans[j].col = mat[i].row;
            fastTrans[j].value = mat[i].value;
        }
    }

    
    cout << "\nOriginal Sparse Matrix (Triplet Form):\n";
    cout << "Row\tCol\tValue\n";
    for (int i = 0; i <= numTerms; i++)
        cout << mat[i].row << "\t" << mat[i].col << "\t" << mat[i].value << endl;

    
    cout << "\nFast Transpose of Sparse Matrix:\n";
    cout << "Row\tCol\tValue\n";
    for (int i = 0; i <= numTerms; i++)
        cout << fastTrans[i].row << "\t" << fastTrans[i].col << "\t" << fastTrans[i].value << endl;

    return 0;
}

```
## Output
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS5.exe'
Enter the number of rows and columns of the matrix: 3 3
Enter the number of non-zero elements: 3
Enter row, column and value for each non-zero element:
0 0 5
0 2 8
2 1 6

Original Sparse Matrix (Triplet Form):
Row     Col     Value
3       3       3
0       0       5
0       2       8
2       1       6

Fast Transpose of Sparse Matrix:
Row     Col     Value
3       3       3
0       0       5
1       2       6
2       0       8


## Conclusion
This implementation efficiently computes the fast transpose of a sparse matrix using its triplet representation. It runs in linear time relative to the number of non-zero elements plus the number of columns, and uses simple arrays to ensure low overhead—making it suitable for large sparse matrices where memory and speed matter.