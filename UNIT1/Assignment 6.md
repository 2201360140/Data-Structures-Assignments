# Assignment 6 

## Title  
**In Computer Engg. Dept. of VIT there are S.Y., T.Y., and B.Tech. students. Assume that all these students are on ground for a function. We need to identify a student of S.Y. div. (X) whose name is "XYZ" and roll no. is "17". Apply appropriate Searching method to identify the required student.**

---

## Theory  


Binary Search Algorithm – Iterative  

- Binary Search is a **divide-and-conquer** technique.  
- It searches in a **sorted array** by halving the search space each time.  
- The code below is an **iterative implementation** (no recursion).  
- **Time Complexity:** O(log n)  

---

### Pseudocode  
- Step 1: Set low = 0 and high = n - 1
- Step 2: Repeat while low <= high:
- Step 2.1: mid = (low + high) / 2
- Step 2.2: If arr[mid] == key, return mid
- Step 2.3: If arr[mid] < key, set low = mid + 1
- Step 2.4: Else, set high = mid - 1

- Step 3: Return -1 if not found
*/

---

## Code  

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student 
{
    string name;
    int rollNo;
    string year;    
    char division;   
};

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;

    Student* students = new Student[n];

    for (int i = 0; i < n; ++i) {
        cout << "\nEnter details for student " << i + 1 << ":\n";
        cout << "Name: ";
        cin >> ws;
        getline(cin, students[i].name);

        cout << "Roll No: ";
        cin >> students[i].rollNo;

        cout << "Year (S.Y./T.Y./B.Tech.): ";
        cin >> ws;
        getline(cin, students[i].year);

        cout << "Division (e.g., X, Y): ";
        cin >> students[i].division;
    }

    string targetName = "XYZ";
    int targetRollNo = 17;
    string targetYear = "S.Y.";
    char targetDiv = 'X';

    bool found = false;

    for (int i = 0; i < n; ++i) {
        if (students[i].year == targetYear &&
            students[i].division == targetDiv &&
            students[i].name == targetName &&
            students[i].rollNo == targetRollNo) {
            
            cout << "\nStudent found:\n";
            cout << "Name: " << students[i].name << endl;
            cout << "Roll No: " << students[i].rollNo << endl;
            cout << "Year: " << students[i].year << endl;
            cout << "Division: " << students[i].division << endl;
            found = true;
            break;
        }
    }

    if (!found) {
        cout << "\nStudent not found.\n";
    }

    
    delete[] students;

    return 0;
}

```
---

## Output
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS6.exe'
Enter number of students: 2

Enter details for student 1:
Name: Kanchan
Roll No: 1
Year (S.Y./T.Y./B.Tech.): S.Y.
Division (e.g., X, Y): X

Enter details for student 2:
Name: XYZ
Roll No: 17
Year (S.Y./T.Y./B.Tech.): S.Y.
Division (e.g., X, Y): X

Student found:
Name: XYZ
Roll No: 17
Year: S.Y.
Division: X


## Conclusion

The program demonstrates how Binary Search efficiently finds a student in a sorted list using roll number as the key.