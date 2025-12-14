# Assignment 2  

## Title  
**Construction and Verification of a Magic Square of Order 'n' (Odd & Even) in C++ Using Dynamic Memory Allocation**  

---

## Theory  
A **magic square** is an \(n \times n\) matrix filled with distinct integers from 1 to \(n^2\) such that the sum of elements in every **row**, **column**, and both **main diagonals** is the same. This sum is called the **magic sum**.  

- For **odd-order squares**, the **Siamese method** (placing numbers in a "move up and right" pattern) is commonly used.  
- For **even-order squares**, there are specific algorithms like **Doubly Even** or **Singly Even methods**, but this program currently handles **odd-order magic squares**.  
- **Dynamic memory allocation** allows the program to create arrays of variable size at runtime.  

---

**Magic Sum Formula:** $\text{Magic Sum} = \frac{n(n^2 + 1)}{2}$

---

## Code  
```cpp
#include <iostream>
#include <vector>
#include <iomanip>
using namespace std;

typedef vector<vector<int>> Matrix;

Matrix generateOddMagicSquare(int n) 
{
    Matrix magic(n, vector<int>(n, 0));
    int num = 1;
    int i = 0, j = n / 2;

    while (num <= n * n) 
    {
        magic[i][j] = num++;
        int newi = (i - 1 + n) % n;
        int newj = (j + 1) % n;

        if (magic[newi][newj])
        {
            i = (i + 1) % n;
        }
        else 
        {
            i = newi;
            j = newj;
        }
    }
    return magic;
}

Matrix generateDoublyEvenMagicSquare(int n) 
{
    Matrix magic(n, vector<int>(n));
    int num = 1;

    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            magic[i][j] = num++;

    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++) 
        {
            if (((i % 4 == j % 4) || ((i + j) % 4 == 3)))
                magic[i][j] = n * n + 1 - magic[i][j];
        }

    return magic;
}

Matrix generateSinglyEvenMagicSquare(int n) 
{
    int half = n / 2;
    Matrix subSquare = generateOddMagicSquare(half);
    Matrix magic(n, vector<int>(n));

    int add = half * half;

    for (int i = 0; i < half; ++i)
        for (int j = 0; j < half; ++j) 
        {
            magic[i][j] = subSquare[i][j];
            magic[i + half][j + half] = subSquare[i][j] + add;
            magic[i][j + half] = subSquare[i][j] + 2 * add;
            magic[i + half][j] = subSquare[i][j] + 3 * add;
        }

    int k = (n - 2) / 4;
    for (int i = 0; i < half; ++i)
        for (int j = 0; j < k; ++j)
            swap(magic[i][j], magic[i + half][j]);

    for (int i = 0; i < half; ++i)
        for (int j = n - k + 1; j < n; ++j)
            swap(magic[i][j], magic[i + half][j]);

    swap(magic[half / 2][k], magic[half / 2 + half][k]);

    return magic;
}

Matrix generateMagicSquare(int n) 
{
    if (n < 3) {
        throw invalid_argument("Order must be >= 3.");
    } else if (n % 2 == 1) {
        return generateOddMagicSquare(n);
    } else if (n % 4 == 0) {
        return generateDoublyEvenMagicSquare(n);
    } else {
        return generateSinglyEvenMagicSquare(n);
    }
}

bool verifyMagicSquare(const Matrix& magic) {
    int n = magic.size();
    int magicSum = n * (n * n + 1) / 2;

    
    for (int i = 0; i < n; ++i) {
        int rowSum = 0, colSum = 0;
        for (int j = 0; j < n; ++j) {
            rowSum += magic[i][j];
            colSum += magic[j][i];
        }
        if (rowSum != magicSum || colSum != magicSum)
            return false;
    }

    
    int diag1 = 0, diag2 = 0;
    for (int i = 0; i < n; ++i) {
        diag1 += magic[i][i];
        diag2 += magic[i][n - 1 - i];
    }
    return diag1 == magicSum && diag2 == magicSum;
}

void printMagicSquare(const Matrix& magic) {
    int n = magic.size();
    cout << "\nMagic Square of order " << n << ":\n";
    for (const auto& row : magic) {
        for (int num : row) {
            cout << setw(4) << num << " ";
        }
        cout << "\n";
    }
}

int main() {
    int n;
    cout << "Enter the order of the magic square (n >= 3): ";
    cin >> n;

    try {
        Matrix magicSquare = generateMagicSquare(n);
        printMagicSquare(magicSquare);

        if (verifyMagicSquare(magicSquare)) {
            cout << "\nThe magic square is valid.\n";
        } else {
            cout << "\nThe magic square is NOT valid.\n";
        }
    } catch (const exception& e) {
        cout << "Error: " << e.what() << "\n";
    }

    return 0;
}

```
## Output:
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'Ass2.exe'
Enter the order of the magic square (n >= 3): 4

Magic Square of order 4:
  16    2    3   13 
   5   11   10    8 
   9    7    6   12 
   4   14   15    1 

The magic square is valid.


## Conclusion

The program constructs a magic square of odd order using dynamic memory. All rows, columns, and diagonals sum to the magic sum, demonstrating array manipulation and algorithmic logic in C++.

---