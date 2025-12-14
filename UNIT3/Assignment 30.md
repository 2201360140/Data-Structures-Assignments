# **Assignment 30**

## **Title**

In a call center, customer calls are handled on a first-come, first-served basis. Implement a queue system using Linked list where:
- Each customer call is enqueued as it arrives
- Customer service agents dequeue calls to assist customers
- If there are no calls, the system waits

## **Theory**

A call center queue system implements the FIFO (First-In-First-Out) principle using a linked list structure to manage incoming customer calls efficiently. The dynamic nature of linked lists allows unlimited calls to be queued while maintaining optimal performance for call routing operations.

**Key Concepts:**

1. **Queue Operations**:
   - **Enqueue Call**: Add incoming call to the rear of the queue
   - **Dequeue Call**: Assign next call from front to available agent
   - **Display Calls**: View all waiting calls in the queue
   - **Wait State**: System handles empty queue gracefully

2. **Linked List Structure**:
   - Each node contains call information (ID, customer name) and pointer to next
   - Front pointer tracks the first call in queue
   - Rear pointer tracks the last call in queue
   - Dynamic memory allocation allows unlimited queue growth

3. **FIFO Behavior**:
   - First call received is first call answered
   - New calls always join at the rear
   - Agents always handle calls from the front
   - Ensures fair service to all customers

4. **Wait State Handling**:
   - When queue is empty, system displays waiting message
   - No calls are lost or skipped
   - System ready to accept new calls immediately

**Algorithm Steps:**

**Enqueue Call (Add Call):**
1. Create new node with call details
2. If queue empty, set front and rear to new node
3. Otherwise, link rear's next to new node and update rear
4. Display call received confirmation

**Dequeue Call (Assign to Agent):**
1. Check if queue is empty
2. If empty, display "No calls waiting" message
3. If not empty, retrieve front call information
4. Move front pointer to next call
5. If queue becomes empty, set rear to nullptr
6. Delete dequeued node and display call details

**Display Waiting Calls:**
1. Check if queue is empty
2. Traverse from front to rear
3. Display all calls with position in queue

## **Code**

```cpp

#include <iostream>
using namespace std;

class Node
{
public:
    int callID;
    Node *next;

    Node(int id)
    {
        callID = id;
        next = nullptr;
    }
};

class Queue
{
public:
    Node *front;
    Node *rear;

    Queue()
    {
        front = nullptr;
        rear = nullptr;
    }

    void enqueue(int callID)
    {
        Node *newNode = new Node(callID);

        if (rear == nullptr)
        {
            front = rear = newNode;
            cout << "Call " << callID << " added to the queue.\n";
            return;
        }

        rear->next = newNode;
        rear = newNode;
        cout << "Call " << callID << " added to the queue.\n";
    }

    void dequeue()
    {
        if (front == nullptr)
        {
            cout << "No calls in the queue. System waiting...\n";
            return;
        }

        Node *temp = front;
        cout << "Call " << temp->callID << " is being attended.\n";

        front = front->next;

        if (front == nullptr)
            rear = nullptr;

        delete temp;
    }

    void display()
    {
        if (front == nullptr)
        {
            cout << "No calls in the queue.\n";
            return;
        }

        Node *temp = front;
        cout << "Current call queue: ";
        while (temp != nullptr)
        {
            cout << temp->callID << " -> ";
            temp = temp->next;
        }
        cout << "NULL\n";
    }
};

int main()
{
    Queue q;
    int choice, callID;

    while (true)
    {
        cout << "\n--- Call Center Queue System ---\n";
        cout << "1. Add Customer Call (Enqueue)\n";
        cout << "2. Assist Next Caller (Dequeue)\n";
        cout << "3. Display Queue\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            cout << "Enter Call ID: ";
            cin >> callID;
            q.enqueue(callID);
            break;

        case 2:
            q.dequeue();
            break;

        case 3:
            q.display();
            break;

        case 4:
            cout << "Exiting...\n";
            return 0;

        default:
            cout << "Invalid choice! Try again.\n";
        }
    }

    return 0;
}

```
## **Output**

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS30.exe'
--- Call Center Queue System ---
1. Add Customer Call (Enqueue)
2. Assist Next Caller (Dequeue)
3. Display Queue
4. Exit
Enter your choice: 1
Enter Call ID: 101
Call 101 added to the queue.

--- Call Center Queue System ---
1. Add Customer Call (Enqueue)
2. Assist Next Caller (Dequeue)
3. Display Queue
4. Exit
Enter your choice: 1
Enter Call ID: 102
Call 102 added to the queue.

--- Call Center Queue System ---
1. Add Customer Call (Enqueue)
2. Assist Next Caller (Dequeue)
3. Display Queue
4. Exit
Enter your choice: 3
Current call queue: 101 -> 102 -> NULL

--- Call Center Queue System ---
1. Add Customer Call (Enqueue)
2. Assist Next Caller (Dequeue)
3. Display Queue
4. Exit
Enter your choice: 2
Call 101 is being attended.

--- Call Center Queue System ---
1. Add Customer Call (Enqueue)
2. Assist Next Caller (Dequeue)
3. Display Queue
4. Exit
Enter your choice: 2
Call 102 is being attended.

--- Call Center Queue System ---
1. Add Customer Call (Enqueue)
2. Assist Next Caller (Dequeue)
3. Display Queue
4. Exit
Enter your choice: 4
Exiting...

## **Conclusion**

The call center queue system efficiently implements FIFO-based call routing using linked list data structure with O(1) time complexity for both enqueue and dequeue operations ensuring instant call handling without performance degradation. The dynamic memory allocation eliminates capacity constraints allowing the system to handle call volume spikes during peak hours while the wait state handling ensures graceful operation when no calls are pending. 