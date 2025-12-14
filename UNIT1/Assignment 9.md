# Assignment 9

## Title
**Write a program using Bubble Sort algorithm to assign roll numbers to students based on their previous year marks. The topper gets Roll No. 1. Analyse the sorting algorithm pass by pass. Use dynamic memory allocation.**

---

## Algorithm

**Title:** Bubble Sort Algorithm – Descending Order  
**Notes:**  
- Bubble Sort is a **simple comparison-based sorting algorithm**.  
- It repeatedly compares **adjacent elements** and swaps them if they are in the wrong order.  
- Here, we sort in **descending order** so that the highest marks come first.  
- After sorting, **roll numbers are reassigned**: topper = roll 1, next highest = roll 2, etc.  
- **Time Complexity:**  
  - Best Case: O(n) (already sorted)  
  - Worst/Average Case: O(n²)

---

## Pseudocode

- Step 1: Input number of students `n`  
- Step 2: Dynamically allocate arrays `marks[n]` and `roll[n]`  
- Step 3: Assign random marks to `marks` array, temporary roll numbers sequentially  
- Step 4: Perform Bubble Sort in descending order:
  - Repeat for pass = 1 to n-1:
    - For i = 0 to n-pass-1:
      - If `marks[i] < marks[i+1]`, swap `marks[i]` and `marks[i+1]`  
    - Display marks after each pass
- Step 5: Reassign roll numbers sequentially according to sorted marks  
- Step 6: Display final sorted list (Marks -> Roll No)  
- Step 7: Free dynamically allocated memory  

---

## Code

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student {
    string name;
    int marks;
};


void swap(Student &a, Student &b) {
    Student temp = a;
    a = b;
    b = temp;
}

void bubbleSort(Student* students, int n) {
    bool swapped;
    for (int i = 0; i < n - 1; i++) {
        swapped = false;

        for (int j = 0; j < n - i - 1; j++) {
            if (students[j].marks < students[j + 1].marks) {
                swap(students[j], students[j + 1]);
                swapped = true;
            }
        }

       
        cout << "Pass " << i + 1 << ": ";
        for (int k = 0; k < n; k++) {
            cout << "(" << students[k].name << ", " << students[k].marks << ") ";
        }
        cout << endl;

        
        if (!swapped)
            break;
    }
}

int main() {
    int n;

    cout << "Enter number of students: ";
    cin >> n;

    
    Student* students = new Student[n];

    
    for (int i = 0; i < n; i++) {
        cout << "Enter name of student " << i + 1 << ": ";
        cin >> students[i].name;
        cout << "Enter marks of " << students[i].name << ": ";
        cin >> students[i].marks;
    }

    cout << "\nSorting students by marks using Bubble Sort...\n";
    bubbleSort(students, n);

   
    cout << "\nAssigned Roll Numbers (Topper = Roll No. 1):\n";
    for (int i = 0; i < n; i++) {
        cout << "Roll No. " << i + 1 << ": " << students[i].name << " (Marks: " << students[i].marks << ")\n";
    }

    
    delete[] students;

    return 0;
}

```
---
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS9.exe'
Enter number of students: 3
Enter name of student 1: Kanchan
Enter marks of Kanchan: 45
Enter name of student 2: Kapil
Enter marks of Kapil: 89
Enter name of student 3: Nehu
Enter marks of Nehu: 56

Sorting students by marks using Bubble Sort...
Pass 1: (Kapil, 89) (Nehu, 56) (Kanchan, 45) 
Pass 2: (Kapil, 89) (Nehu, 56) (Kanchan, 45) 

Assigned Roll Numbers (Topper = Roll No. 1):
Roll No. 1: Kapil (Marks: 89)
Roll No. 2: Nehu (Marks: 56)
Roll No. 3: Kanchan (Marks: 45)

---

## Conclusion
The program sorts students’ marks in descending order and assigns roll numbers accordingly, ensuring the topper gets roll 1. Pass-by-pass analysis shows how Bubble Sort gradually orders the array. Although simple, Bubble Sort has O(n²) time complexity, making it less efficient for large datasets.