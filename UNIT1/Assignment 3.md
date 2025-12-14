# Assignment 3  

## Title  
**Implementation of Matrix Multiplication and Analysis of Row-Major vs Column-Major Access in C++**

---

## Theory  
Matrix multiplication is a computationally intensive operation widely used in scientific and engineering applications.  
For two square matrices \( A_{n \times n} \) and \( B_{n \times n} \), the resulting matrix  
\( C_{n \times n} \) is computed as:

\[
C[i][j] = \sum_{k=0}^{n-1} A[i][k] \times B[k][j]
\]

In this program, matrix multiplication is implemented using two different **loop traversal orders** to study the effect of **memory access patterns** on execution time in C++.

- **Row-Major Multiplication:**  
  The outer loops iterate over rows first (`i → j → k`). Since C++ stores 2D arrays in row-major order, consecutive memory locations are accessed, leading to better cache utilization.

- **Column-Major Multiplication:**  
  The outer loops iterate over columns first (`j → i → k`). This access pattern does not follow the natural memory layout of C++, resulting in non-contiguous memory access and potentially more cache misses.

The execution time for both approaches is measured using the `chrono` library, allowing a practical comparison of their performance.

---

## Example Matrix (3×3)

|     | 0 | 1 | 2 |
|-----|---|---|---|
| 0   | 1 | 2 | 3 |
| 1   | 4 | 5 | 6 |
| 2   | 7 | 8 | 9 |

---

## Row-Major Access Order  

Traversal is performed **row-wise**, matching C++’s memory layout:

| Step | Element Accessed | Memory Index |
|------|-----------------|--------------|
| 1    | 1               | 0            |
| 2    | 2               | 1            |
| 3    | 3               | 2            |
| 4    | 4               | 3            |
| 5    | 5               | 4            |
| 6    | 6               | 5            |
| 7    | 7               | 6            |
| 8    | 8               | 7            |
| 9    | 9               | 8            |

This sequential access improves cache locality and generally reduces execution time.

---

## Column-Major Access Order  

Traversal is performed **column-wise**, which is less suitable for row-major memory layout:

| Step | Element Accessed | Memory Index |
|------|-----------------|--------------|
| 1    | 1               | 0            |
| 2    | 4               | 1            |
| 3    | 7               | 2            |
| 4    | 2               | 3            |
| 5    | 5               | 4            |
| 6    | 8               | 5            |
| 7    | 3               | 6            |
| 8    | 6               | 7            |
| 9    | 9               | 8            |

This pattern can cause inefficient cache usage due to non-contiguous memory access.

---

## Code  
```cpp
#include <iostream>
#include <vector>
#include <chrono>
using namespace std;
using namespace std::chrono;


void initializeMatrix(vector<vector<double>>& mat, int n) {
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            mat[i][j] = (i + j) % 100;
}


void multiplyRowMajor(const vector<vector<double>>& A, const vector<vector<double>>& B, vector<vector<double>>& C, int n) {
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            for (int k = 0; k < n; ++k)
                C[i][j] += A[i][k] * B[k][j];
}


void multiplyColMajor(const vector<vector<double>>& A, const vector<vector<double>>& B, vector<vector<double>>& C, int n) {
    for (int j = 0; j < n; ++j)
        for (int i = 0; i < n; ++i)
            for (int k = 0; k < n; ++k)
                C[i][j] += A[i][k] * B[k][j];
}

//Display row wise and column wise Multiplication
/*void printMatrix(const vector<vector<double>>& M, int n) {
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j)
            cout << M[i][j] << " ";
        cout << endl;
    }
}*/

int main() {
    int n;
    cout << "Enter matrix size (n x n): ";
    cin >> n;

    vector<vector<double>> A(n, vector<double>(n));
    vector<vector<double>> B(n, vector<double>(n));
    vector<vector<double>> C1(n, vector<double>(n, 0));
    vector<vector<double>> C2(n, vector<double>(n, 0));

    initializeMatrix(A, n);
    initializeMatrix(B, n);

    auto start1 = high_resolution_clock::now();
    multiplyRowMajor(A, B, C1, n);
    auto end1 = high_resolution_clock::now();
    auto duration1 = duration_cast<milliseconds>(end1 - start1);
    cout << "Row-major multiplication time: " << duration1.count() << " ms" << endl;

    auto start2 = high_resolution_clock::now();
    multiplyColMajor(A, B, C2, n);
    auto end2 = high_resolution_clock::now();
    auto duration2 = duration_cast<milliseconds>(end2 - start2);
    cout << "Column-major multiplication time: " << duration2.count() << " ms" << endl;

   /* cout << "\nMatrix C1 (Row-major result):\n";
    printMatrix(C1, n);

    cout << "\nMatrix C2 (Column-major result):\n";
    printMatrix(C2, n);*/


    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS3.exe'
Enter matrix size (n x n): 3 3
Row-major multiplication time: 2 ms
Column-major multiplication time: 0 ms

## Conclusion

The program compares row-major and column-major matrix multiplication in C++.  
Although both approaches produce the same result, their execution times differ due to memory access patterns.  
Row-major traversal aligns with C++’s internal memory layout and is generally more cache-efficient.  
For small matrix sizes, the time difference may be minimal or negligible, but for larger matrices, row-major access typically provides better performance.

---