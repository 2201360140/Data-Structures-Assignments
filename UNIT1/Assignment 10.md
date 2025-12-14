# Assignment 10

## Title  
**Write a program to arrange the list of employees as per the average of their height and weight by using Merge and Selection sorting method. Analyse their time complexities and conclude which algorithm will take less time to sort the list.**

---

## Algorithm  

### 1. Selection Sort Algorithm – Iterative  
**Notes:**  
- Selection Sort is a **simple comparison-based algorithm**.  
- Finds the minimum element from the unsorted part and moves it to the beginning.  
- **Time Complexity:** O(n²)  

**Pseudocode:**  
- Step 1: For i = 0 to n-2  
- Step 2: Set minIndex = i  
- Step 3: For j = i+1 to n-1  
- Step 3.1: If arr[j].avg < arr[minIndex].avg, set minIndex = j  
- Step 4: Swap arr[i] with arr[minIndex]  
- Step 5: Repeat until array is sorted  

---

### 2. Merge Sort Algorithm – Recursive  
**Notes:**  
- Merge Sort is a **divide-and-conquer algorithm**.  
- Divides the array into halves, sorts them recursively, and merges the sorted halves.  
- **Time Complexity:** O(n log n)  

**Pseudocode:**  
- Step 1: If left < right  
- Step 2: mid = (left + right) / 2  
- Step 3: Recursively sort left half (left to mid)  
- Step 4: Recursively sort right half (mid+1 to right)  
- Step 5: Merge the two sorted halves  

---

## C++ Code  

```cpp
#include <iostream>
#include <iomanip>
#include <string>
using namespace std;

struct Employee {
    string name;
    float height;
    float weight;
    float average;
};

void calculateAverage(Employee* emp, int n) {
    for (int i = 0; i < n; i++) {
        emp[i].average = (emp[i].height + emp[i].weight) / 2.0;
    }
}


void selectionSort(Employee* emp, int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;

        for (int j = i + 1; j < n; j++) {
            if (emp[j].average < emp[minIndex].average) {
                minIndex = j;
            }
        }

        if (minIndex != i) {
            swap(emp[i], emp[minIndex]);
        }
    }
}


void merge(Employee* emp, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    Employee* L = new Employee[n1];
    Employee* R = new Employee[n2];

    for (int i = 0; i < n1; i++)
        L[i] = emp[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = emp[mid + 1 + j];

    int i = 0, j = 0, k = left;

    while (i < n1 && j < n2) {
        if (L[i].average <= R[j].average)
            emp[k++] = L[i++];
        else
            emp[k++] = R[j++];
    }

    while (i < n1)
        emp[k++] = L[i++];

    while (j < n2)
        emp[k++] = R[j++];

    delete[] L;
    delete[] R;
}

void mergeSort(Employee* emp, int left, int right) {
    if (left < right) {
        int mid = (left + right) / 2;
        mergeSort(emp, left, mid);
        mergeSort(emp, mid + 1, right);
        merge(emp, left, mid, right);
    }
}


void displayEmployees(Employee* emp, int n) {
    cout << fixed << setprecision(2);
    cout << "\nName\tHeight\tWeight\tAverage\n";
    cout << "-----------------------------------\n";
    for (int i = 0; i < n; i++) {
        cout << emp[i].name << "\t" << emp[i].height << "\t" << emp[i].weight << "\t" << emp[i].average << endl;
    }
}


int main() {
    int n;
    cout << "Enter number of employees: ";
    cin >> n;

    Employee* empList = new Employee[n];
    Employee* empListForSelection = new Employee[n]; 
    
    for (int i = 0; i < n; i++) {
        cout << "\nEnter name of employee " << i + 1 << ": ";
        cin >> empList[i].name;
        cout << "Enter height (in cm) of " << empList[i].name << ": ";
        cin >> empList[i].height;
        cout << "Enter weight (in kg) of " << empList[i].name << ": ";
        cin >> empList[i].weight;
    }

    
    for (int i = 0; i < n; i++) {
        empListForSelection[i] = empList[i];
    }

    
    calculateAverage(empList, n);
    calculateAverage(empListForSelection, n);

   
    mergeSort(empList, 0, n - 1);
    cout << "\nSorted Employees by Average (Merge Sort):";
    displayEmployees(empList, n);

    
    selectionSort(empListForSelection, n);
    cout << "\nSorted Employees by Average (Selection Sort):";
    displayEmployees(empListForSelection, n);

    
    delete[] empList;
    delete[] empListForSelection;

    return 0;
}

```
---
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS10.exe'
Enter number of employees: 3

Enter name of employee 1: Kanchan
Enter height (in cm) of Kanchan: 170
Enter weight (in kg) of Kanchan: 46

Enter name of employee 2: Kapil
Enter height (in cm) of Kapil: 190
Enter weight (in kg) of Kapil: 80

Enter name of employee 3: Pogo
Enter height (in cm) of Pogo: 150
Enter weight (in kg) of Pogo: 45

Sorted Employees by Average (Merge Sort):
Name    Height  Weight  Average
-----------------------------------
Pogo    150.00  45.00   97.50
Kanchan 170.00  46.00   108.00
Kapil   190.00  80.00   135.00

Sorted Employees by Average (Selection Sort):
Name    Height  Weight  Average
-----------------------------------
Pogo    150.00  45.00   97.50
Kanchan 170.00  46.00   108.00
Kapil   190.00  80.00   135.00

---
## Conclusion

Selection Sort works well for small datasets but becomes inefficient for larger datasets due to O(n²) complexity. Merge Sort is more efficient for larger datasets with O(n log n) complexity. 

---