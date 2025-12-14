# Assignment 4  

## Title  
**Implementation of Sparse Matrix using Compact Representation and its Transpose in C++**

---

## Theory  
A **sparse matrix** is a matrix in which most elements are zero. Storing all elements directly wastes memory, so we use **compact representation**, storing only **non-zero elements** with their row and column indices.  

- **Compact Representation:** Store each non-zero element as `(row, column, value)`.  
- **Transpose:** Swap row and column indices in the compact form to get the transpose efficiently without constructing the full matrix.  
- **Dynamic Memory Allocation:** Used to handle matrices of arbitrary size at runtime.

---

## Code  
```cpp
#include <iostream>
#include <vector>
using namespace std;

struct Element {
    int row;
    int col;
    int value;
};

vector<Element> convertToCompact(const vector<vector<int>>& mat, int rows, int cols) {
    vector<Element> compact;

    
    int count = 0;
    for (int i = 0; i < rows; ++i)
        for (int j = 0; j < cols; ++j)
            if (mat[i][j] != 0)
                count++;

    compact.push_back({rows, cols, count});

    for (int i = 0; i < rows; ++i)
        for (int j = 0; j < cols; ++j)
            if (mat[i][j] != 0)
                compact.push_back({i, j, mat[i][j]});

    return compact;
}


void displayMatrix(const vector<vector<int>>& mat) {
    cout << "Full Matrix:\n";
    for (const auto& row : mat) {
        for (int val : row)
            cout << val << " ";
        cout << "\n";
    }
}


void displayCompact(const vector<Element>& compact) {
    cout << "\nCompact Representation (row col value):\n";
    for (const auto& e : compact)
        cout << e.row << " " << e.col << " " << e.value << "\n";
}


vector<Element> transposeCompact(const vector<Element>& compact) {
    vector<Element> transposed;

    int rows = compact[0].row;
    int cols = compact[0].col;
    int count = compact[0].value;

    transposed.push_back({cols, rows, count}); // Metadata

    
    for (size_t i = 1; i < compact.size(); ++i) {
        transposed.push_back({compact[i].col, compact[i].row, compact[i].value});
    }

    return transposed;
}

int main() {
    int rows, cols;

    cout << "Enter number of rows and columns: ";
    cin >> rows >> cols;

    vector<vector<int>> matrix(rows, vector<int>(cols));

    cout << "Enter elements of the matrix:\n";
    for (int i = 0; i < rows; ++i)
        for (int j = 0; j < cols; ++j)
            cin >> matrix[i][j];

    displayMatrix(matrix);

    
    int nonZero = 0;
    for (auto& row : matrix)
        for (int val : row)
            if (val != 0)
                nonZero++;

    if (nonZero > (rows * cols) / 2) {
        cout << "\nThis is NOT a sparse matrix.\n";
    } else {
        cout << "\nThis is a sparse matrix.\n";

        
        vector<Element> compact = convertToCompact(matrix, rows, cols);
        displayCompact(compact);

        
        vector<Element> transposed = transposeCompact(compact);
        cout << "\nTransposed Compact Representation (row col value):\n";
        displayCompact(transposed);
    }

    return 0;
}

```
## Output
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS4.exe'
Enter number of rows and columns:  4 4
Enter elements of the matrix:
0 0 8 9
0 0 1 0
1 2 0 0
0 0 0 0
Full Matrix:
0 0 8 9 
0 0 1 0 
1 2 0 0 
0 0 0 0 

This is a sparse matrix.

Compact Representation (row col value):
4 4 5
0 2 8
0 3 9
1 2 1
2 0 1
2 1 2

Transposed Compact Representation (row col value):

Compact Representation (row col value):
4 4 5
2 0 8
3 0 9
2 1 1
0 2 1
1 2 2

---
## Conclusion

The program demonstrates how a sparse matrix can be stored efficiently using compact representation and how its transpose can be obtained directly from this representation, reducing memory usage compared to a full 2D array.

---