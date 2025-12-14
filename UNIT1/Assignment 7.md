# Assignment 4  

## Title  
**Implementation of Bubble Sort and Quick Sort on Student Structure using Roll Number as Key in C++**  

---

## Theory  
Sorting is a fundamental operation in data structures and algorithms. Here, we apply **Bubble Sort** and **Quick Sort** to a 1D array of **Student structure**, where each student has a **name, roll number, and marks**. The key for sorting is the **roll number**.  

- **Bubble Sort:**  
  A simple comparison-based sorting algorithm. Adjacent elements are repeatedly compared, and if they are in the wrong order, they are swapped. The process continues until the array is sorted.  
  - *Time Complexity:* O(n²)  
  - *Space Complexity:* O(1)  

- **Quick Sort:**  
  A divide-and-conquer algorithm. It selects a **pivot**, partitions the array into two parts (elements smaller than pivot and elements larger than pivot), and recursively sorts each part.  
  - *Time Complexity:* O(n log n) on average, O(n²) in the worst case  
  - *Space Complexity:* O(log n) due to recursion  

- **Swap Count:**  
  To compare the efficiency of sorting algorithms, we count the number of swaps performed by each algorithm.  

---

## Algorithm  

### Bubble Sort Algorithm  
1. Start with an array of `n` students.  
2. Repeat for `i = 0` to `n-2`:  
   - For `j = 0` to `n-i-2`:  
     - Compare roll numbers of `arr[j]` and `arr[j+1]`.  
     - If `arr[j].roll > arr[j+1].roll`, swap them and increment swap counter.  
3. Repeat steps until the array is sorted.  
4. Display sorted list and number of swaps.  

---

### Quick Sort Algorithm  
1. Choose a pivot element (last element of the array).  
2. Partition the array:  
   - Place all elements with roll number ≤ pivot to the left.  
   - Place all elements with roll number > pivot to the right.  
   - Count swaps whenever two students are exchanged.  
3. Recursively apply Quick Sort to:  
   - Left subarray (elements before pivot).  
   - Right subarray (elements after pivot).  
4. Continue until the base case (`low >= high`).  
5. Display sorted list and number of swaps.  

---

## Code  
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student
{
    string name;
    int roll_no;
    float total_marks;
};

void swap(Student &a, Student &b, int &swapCount)
{
    Student temp = a;
    a = b;
    b = temp;
    swapCount++;
}

void bubbleSort(Student* arr, int n, int &swapCount)
{
    swapCount = 0; 
    for (int i = 0; i < n - 1; ++i)
    {
        for (int j = 0; j < n - i - 1; ++j) 
        {
            if (arr[j].roll_no > arr[j + 1].roll_no) 
            {
                swap(arr[j], arr[j + 1], swapCount);
            }
        }
    }
}


int partition(Student* arr, int low, int high, int &swapCount) {
    int pivot = arr[high].roll_no;
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j].roll_no < pivot) {
            i++;
            swap(arr[i], arr[j], swapCount);
        }
    }
    swap(arr[i + 1], arr[high], swapCount);
    return i + 1;
}

void quickSort(Student* arr, int low, int high, int &swapCount) {
    if (low < high) {
        int pi = partition(arr, low, high, swapCount);
        quickSort(arr, low, pi - 1, swapCount);
        quickSort(arr, pi + 1, high, swapCount);
    }
}


void display(Student* arr, int n) {
    cout << "\nSorted Student List (by Roll No):\n";
    for (int i = 0; i < n; ++i) {
        cout << "Name: " << arr[i].name
             << ", Roll No: " << arr[i].roll_no
             << ", Total Marks: " << arr[i].total_marks << endl;
    }
}

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;

    Student* students1 = new Student[n]; 
    Student* students2 = new Student[n];

    for (int i = 0; i < n; ++i) {
        cout << "\nEnter details for student " << i + 1 << ":\n";
        cout << "Name: ";
        cin >> ws;
        getline(cin, students1[i].name);
        cout << "Roll No: ";
        cin >> students1[i].roll_no;
        cout << "Total Marks: ";
        cin >> students1[i].total_marks;

        students2[i] = students1[i];
    }

    int bubbleSwapCount = 0;
    int quickSwapCount = 0;

   
    bubbleSort(students1, n, bubbleSwapCount);
    display(students1, n);
    cout << "Bubble Sort Swaps: " << bubbleSwapCount << endl;

   
    quickSort(students2, 0, n - 1, quickSwapCount);
    display(students2, n);
    cout << "Quick Sort Swaps: " << quickSwapCount << endl;

    
    delete[] students1;
    delete[] students2;

    return 0;
}

```
---
## Output 

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS7.exe'
Enter number of students: 3

Enter details for student 1:
Name: Kanchan
Roll No: 17
Total Marks: 94

Enter details for student 2:
Name: Pogo
Roll No: 15
Total Marks: 95

Enter details for student 3:
Name: Kapil
Roll No: 45
Total Marks: 76

Sorted Student List (by Roll No):
Name: Pogo, Roll No: 15, Total Marks: 95
Name: Kanchan, Roll No: 17, Total Marks: 94
Name: Kapil, Roll No: 45, Total Marks: 76
Bubble Sort Swaps: 1

Sorted Student List (by Roll No):
Name: Pogo, Roll No: 15, Total Marks: 95
Name: Kanchan, Roll No: 17, Total Marks: 94
Name: Kapil, Roll No: 45, Total Marks: 76
Quick Sort Swaps: 4

---


## Conclusion

Bubble Sort is simple but inefficient for large datasets due to its **O(n²)** complexity, while Quick Sort is much faster with an average complexity of **O(n log n)**.  The swap count comparison clearly shows Quick Sort’s efficiency over Bubble Sort in practice.  

---