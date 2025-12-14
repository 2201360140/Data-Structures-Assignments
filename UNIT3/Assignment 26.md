# **Assignment 26**

## **Title**

Write a program to keep track of patients as they checked into a medical clinic, assigning patients to doctors on a first-come, first-served basis.

## **Theory**

A queue is a linear data structure that follows the First-In-First-Out (FIFO) principle, making it ideal for managing patient check-ins at a medical clinic. Using a linked list implementation provides dynamic memory allocation, allowing the queue to grow and shrink as patients arrive and are assigned to doctors.

**Key Concepts:**

1. **Queue Operations**:
   - **Enqueue**: Add a patient to the rear of the queue
   - **Dequeue**: Remove and assign the patient from the front of the queue
   - **Display**: View all patients currently waiting
   - **isEmpty**: Check if there are any patients waiting

2. **FIFO Principle**:
   - First patient to arrive is the first to see the doctor
   - New patients are added at the rear (end) of the queue
   - Patients are called from the front (beginning) of the queue

3. **Linked List Implementation**:
   - Each node contains patient information and a pointer to the next patient
   - Front pointer points to the first patient in line
   - Rear pointer points to the last patient in line
   - Dynamic size allows unlimited patient queue

**Algorithm Steps:**

**Enqueue Operation (Add Patient):**
1. Create a new node with patient information
2. If queue is empty, set both front and rear to new node
3. Otherwise, link rear's next to new node and update rear
4. Display confirmation message

**Dequeue Operation (Assign to Doctor):**
1. Check if queue is empty
2. If empty, display "No patients waiting" message
3. If not empty, retrieve front patient's information
4. Move front pointer to next patient
5. If queue becomes empty, set rear to nullptr
6. Delete the dequeued node and display patient info

**Display Operation:**
1. Check if queue is empty
2. Traverse from front to rear
3. Display each patient's information

## **Code**

```cpp
#include <iostream>
#include <queue>
#include <string>
using namespace std;

int main() {
    queue<string> waitingList;
    int choice;
    string name;

    while (true) {
        cout << "\n==== Medical Clinic System ====\n";
        cout << "1. Check-in patient\n";
        cout << "2. Assign patient to doctor\n";
        cout << "3. View waiting patients\n";
        cout << "4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch(choice) {
            case 1:
                cout << "Enter patient name: ";
                cin >> name;
                waitingList.push(name);
                cout << "Patient " << name << " checked in.\n";
                break;

            case 2:
                if (waitingList.empty()) {
                    cout << "No patients waiting.\n";
                } else {
                    cout << "Assigned patient " << waitingList.front() << " to doctor.\n";
                    waitingList.pop();
                }
                break;

            case 3:
                if (waitingList.empty()) {
                    cout << "No patients in waiting list.\n";
                } else {
                    cout << "Patients in queue:\n";
                    queue<string> temp = waitingList;  
                    while (!temp.empty()) {
                        cout << "- " << temp.front() << endl;
                        temp.pop();
                    }
                }
                break;

            case 4:
                cout << "Exiting system.\n";
                return 0;

            default:
                cout << "Invalid choice! Please try again.\n";
        }
    }

    return 0;
}

```
## **Output**

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS26.exe'
==== Medical Clinic System ====
1. Check-in patient
2. Assign patient to doctor
3. View waiting patients
4. Exit
Enter choice: 1
Enter patient name: Kanchan
Patient Kanchan checked in.

==== Medical Clinic System ====
1. Check-in patient
2. Assign patient to doctor
3. View waiting patients
4. Exit
Enter choice: 1
Enter patient name: Kapil
Patient Kapil checked in.

==== Medical Clinic System ====
1. Check-in patient
2. Assign patient to doctor
3. View waiting patients
4. Exit
Enter choice: 3
Patients in queue:
- Kanchan
- Kapil

==== Medical Clinic System ====
1. Check-in patient
2. Assign patient to doctor
3. View waiting patients
4. Exit
Enter choice: 2
Assigned patient Kanchan to doctor.

==== Medical Clinic System ====
1. Check-in patient
2. Assign patient to doctor
3. View waiting patients
4. Exit
Enter choice: 2
Assigned patient Kapil to doctor.

==== Medical Clinic System ====
1. Check-in patient
2. Assign patient to doctor
3. View waiting patients
4. Exit
Enter choice: 4
Exiting system.

## **Conclusion**

The patient queue management system successfully demonstrates the practical application of queue data structure in real-world scenarios where fairness and order are paramount. This implementation uses a linked list-based queue that provides efficient O(1) time complexity for both enqueue and dequeue operations, ensuring that patient check-ins and doctor assignments happen instantly regardless of queue size. 