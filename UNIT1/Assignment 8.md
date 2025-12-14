# Assignment 8

## Title
**Write a program to input marks of n students, sort them in ascending order using Quick Sort without using built-in functions, and find minimum and maximum marks using Divide and Conquer. Analyse pass by pass.**

---

## Algorithm

**Quick Sort Algorithm – Ascending Order**  
- Quick Sort is a **divide-and-conquer** sorting algorithm.  
- Select a **pivot** (last element) and partition the array:  
  - Elements ≤ pivot go to the left  
  - Elements > pivot go to the right  
- Recursively apply Quick Sort on left and right subarrays.  
- Time Complexity:  
  - Best/Average Case: O(n log n)  
  - Worst Case: O(n²)  

**Recursive Min-Max Algorithm**  
- Recursively divide the array into halves.  
- If only one element → return as min and max.  
- If two elements → compare and return min and max.  
- Combine left and right halves’ results to get overall min and max.  

---

## Pseudocode

**Quick Sort (Ascending)**  
- Step 1: If low < high:  
- Step 2: Partition array around pivot, get partition index `pi`  
- Step 3: Display array after partition (pass-by-pass)  
- Step 4: Recursively quicksort left (low to pi-1) and right (pi+1 to high)  

**Recursive Min-Max**  
- Step 1: If one element → min = max = element  
- Step 2: If two elements → compare and assign min and max  
- Step 3: Else divide array into left and right halves  
- Step 4: Recursively find min-max of both halves  
- Step 5: Assign overall min = min(left, right), max = max(left, right)

---

## Code

```cpp
#include <iostream>
using namespace std;


int partition(int* arr, int low, int high, int& pass) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }

    swap(arr[i + 1], arr[high]);

    
    cout << "Pass " << pass++ << ": ";
    for (int k = 0; k <= high; k++) {
        cout << arr[k] << " ";
    }
    cout << endl;

    return i + 1;
}

void quickSort(int* arr, int low, int high, int& pass) {
    if (low < high) {
        int pi = partition(arr, low, high, pass);
        quickSort(arr, low, pi - 1, pass);
        quickSort(arr, pi + 1, high, pass);
    }
}


void findMinMax(int* arr, int low, int high, int& minVal, int& maxVal) {
    if (low == high) {
        if (arr[low] < minVal) minVal = arr[low];
        if (arr[low] > maxVal) maxVal = arr[low];
        return;
    }
    else if (high == low + 1) {
        if (arr[low] < arr[high]) {
            if (arr[low] < minVal) minVal = arr[low];
            if (arr[high] > maxVal) maxVal = arr[high];
        } else {
            if (arr[high] < minVal) minVal = arr[high];
            if (arr[low] > maxVal) maxVal = arr[low];
        }
        return;
    }

    int mid = (low + high) / 2;
    findMinMax(arr, low, mid, minVal, maxVal);
    findMinMax(arr, mid + 1, high, minVal, maxVal);
}


int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;

    
    int* marks = new int[n];

    for (int i = 0; i < n; i++) {
        cout << "Enter marks for student " << i + 1 << ": ";
        cin >> marks[i];
    }

    cout << "\nOriginal Marks: ";
    for (int i = 0; i < n; i++) {
        cout << marks[i] << " ";
    }
    cout << endl;

    
    int pass = 1;
    quickSort(marks, 0, n - 1, pass);

    cout << "\nSorted Marks: ";
    for (int i = 0; i < n; i++) {
        cout << marks[i] << " ";
    }
    cout << endl;

    
    int minVal = marks[0], maxVal = marks[0];
    findMinMax(marks, 0, n - 1, minVal, maxVal);

    cout << "\nMinimum Marks: " << minVal << endl;
    cout << "Maximum Marks: " << maxVal << endl;

    delete[] marks;

    return 0;
}

```
## Output
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS8.exe'
Enter number of students: 3
Enter marks for student 1: 75
Enter marks for student 2: 87
Enter marks for student 3: 90

Original Marks: 75 87 90 
Pass 1: 75 87 90 
Pass 2: 75 87 

Sorted Marks: 75 87 90 

Minimum Marks: 75
Maximum Marks: 90

---

## Conclusion
The program sorts student marks using Quick Sort and finds minimum and maximum using recursive Divide and Conquer. Pass-by-pass display shows the gradual ordering of the array. Dynamic memory allocation ensures memory efficiency.

---