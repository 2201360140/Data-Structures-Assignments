# **Assignment 28**

## **Title**

Write a program that maintains a queue of passengers waiting to see a ticket agent. The program user should be able to insert a new passenger at the rear of the queue, Display the passenger at the front of the Queue, or remove the passenger at the front of the queue. The program will display the number of passengers left in the queue just before it terminates.

## **Theory**

A passenger queue system implements the FIFO (First-In-First-Out) principle using a linked list structure to manage passengers waiting to see a ticket agent. The dynamic nature of linked lists allows unlimited passengers to join the queue while maintaining efficient insertion and removal operations.

**Key Concepts:**

1. **Queue Operations**:
   - **Insert**: Add passenger at the rear of the queue
   - **Display Front**: View passenger details at the front without removing
   - **Remove**: Dequeue the passenger at the front
   - **Count**: Track the total number of passengers in queue

2. **Linked List Structure**:
   - Each node contains passenger information and pointer to next
   - Front pointer tracks the first passenger in line
   - Rear pointer tracks the last passenger in line
   - Counter variable maintains queue size

3. **FIFO Behavior**:
   - New passengers always join at the rear
   - Ticket agent always serves from the front
   - Maintains fair ordering of service

**Algorithm Steps:**

**Insert Passenger:**
1. Create new node with passenger details
2. If queue empty, set front and rear to new node
3. Otherwise, link rear's next to new node and update rear
4. Increment passenger count

**Display Front Passenger:**
1. Check if queue is empty
2. If not empty, display front passenger information
3. Do not remove or modify the queue

**Remove Passenger:**
1. Check if queue is empty
2. Store front passenger information
3. Move front pointer to next passenger
4. If queue becomes empty, set rear to nullptr
5. Delete removed node and decrement count

## **Code**

```cpp
#include <iostream>
#include <queue>
#include <string>
using namespace std;

int main()
{
    queue<string> passengers;
    int choice;
    string name;

    while (true)
    {
        cout << "\n====== Ticket Agent Queue System ======\n";
        cout << "1. Add Passenger to Queue\n";
        cout << "2. Show Passenger at Front\n";
        cout << "3. Remove Passenger from Front\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {

        case 1:
            cout << "Enter passenger name: ";
            cin >> name;
            passengers.push(name);
            cout << name << " added to the queue.\n";
            break;

        case 2:
            if (passengers.empty())
                cout << "No passengers in queue.\n";
            else
                cout << "Passenger at front: " << passengers.front() << endl;
            break;

        case 3:
            if (passengers.empty())
                cout << "No passengers to remove.\n";
            else
            {
                cout << passengers.front() << " removed from the queue.\n";
                passengers.pop();
            }
            break;

        case 4:
            cout << "\nProgram ending..." << endl;
            cout << "Passengers left in queue: " << passengers.size() << endl;
            return 0;

        default:
            cout << "Invalid choice! Try again.\n";
        }
    }
}

```
## **Output**

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS28.exe'
====== Ticket Agent Queue System ======
1. Add Passenger to Queue
2. Show Passenger at Front
3. Remove Passenger from Front
4. Exit
Enter your choice: 1
Enter passenger name: Kanchan
Kanchan added to the queue.

====== Ticket Agent Queue System ======
1. Add Passenger to Queue
2. Show Passenger at Front
3. Remove Passenger from Front
4. Exit
Enter your choice: 1
Enter passenger name: Kapil
Kapil added to the queue.

====== Ticket Agent Queue System ======
1. Add Passenger to Queue
2. Show Passenger at Front
3. Remove Passenger from Front
4. Exit
Enter your choice: 1
Enter passenger name: Dipali
Dipali added to the queue.

====== Ticket Agent Queue System ======
1. Add Passenger to Queue
2. Show Passenger at Front
3. Remove Passenger from Front
4. Exit
Enter your choice: 2
Passenger at front: Kanchan

====== Ticket Agent Queue System ======
1. Add Passenger to Queue
2. Show Passenger at Front
3. Remove Passenger from Front
4. Exit
Enter your choice: 3
Kanchan removed from the queue.

====== Ticket Agent Queue System ======
1. Add Passenger to Queue
2. Show Passenger at Front
3. Remove Passenger from Front
4. Exit
Enter your choice: 4
Program ending...
Passengers left in queue: 2

## **Conclusion**

The ticket agent passenger queue system effectively implements a FIFO-based service mechanism using linked list data structure, providing O(1) time complexity for insertion at rear and removal from front operations while maintaining dynamic memory allocation for unlimited passenger capacity.