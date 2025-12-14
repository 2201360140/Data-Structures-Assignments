# **Assignment 29**

## **Title**

Write a program to implement multiple queues i.e. two queues using array and perform following operations on it:
- A. Add Queue
- B. Delete from Queue
- C. Display Queue

## **Theory**

Multiple queues can be efficiently implemented in a single array by dividing the array into equal segments, where each segment represents one independent queue. This approach optimizes memory usage and allows multiple queues to operate simultaneously without interference.

**Key Concepts:**

1. **Array Division Strategy**:
   - Divide array into k equal parts for k queues
   - Each queue gets size = total_size / number_of_queues
   - Queue i occupies indices from (i * queue_size) to ((i+1) * queue_size - 1)

2. **Front and Rear Management**:
   - Each queue maintains its own front and rear pointers
   - Front points to the first element position
   - Rear points to the last element position
   - Initially front = rear = -1 (empty queue)

3. **Queue Operations**:
   - **Add (Enqueue)**: Insert element at rear+1 position
   - **Delete (Dequeue)**: Remove element from front position
   - **Display**: Show all elements from front to rear

4. **Index Calculation**:
   - Base index for queue i = i * queue_size
   - Actual array index = base_index + relative_position
   - This maps logical queue position to physical array location

**Algorithm Steps:**

**Add to Queue (Enqueue):**
1. Check if queue is full (rear == queue_size - 1)
2. If full, display overflow message
3. If empty (front == -1), set front = 0
4. Increment rear and insert element at calculated index
5. Display success message

**Delete from Queue (Dequeue):**
1. Check if queue is empty (front == -1 or front > rear)
2. If empty, display underflow message
3. Retrieve element from front position
4. Increment front pointer
5. If queue becomes empty, reset front = rear = -1
6. Display deleted element

**Display Queue:**
1. Check if queue is empty
2. Traverse from front to rear
3. Display all elements in the queue

## **Code**

```cpp
#include <iostream>
using namespace std;

class TwoQueues
{
private:
    int size;
    int *arr;
    int front1, rear1;
    int front2, rear2;
    int mid; 

public:
    TwoQueues(int n)
    {
        size = n;
        arr = new int[n];
        mid = size / 2;

        front1 = rear1 = -1;
        front2 = rear2 = mid - 1;
    }

    bool isFull1() { return rear1 == mid - 1; }
    bool isEmpty1() { return (front1 == -1) || (front1 > rear1); }

    bool isFull2() { return rear2 == size - 1; }
    bool isEmpty2() { return (front2 == mid - 1) || (front2 > rear2); }

    void enqueue1(int x)
    {
        if (isFull1())
        {
            cout << "Queue 1 Overflow!\n";
            return;
        }
        if (front1 == -1)
            front1 = 0;
        arr[++rear1] = x;
        cout << x << " inserted in Queue 1.\n";
    }

    void enqueue2(int x)
    {
        if (isFull2())
        {
            cout << "Queue 2 Overflow!\n";
            return;
        }
        if (front2 == mid - 1)
            front2 = mid; 
        arr[++rear2] = x;
        cout << x << " inserted in Queue 2.\n";
    }

    void dequeue1()
    {
        if (isEmpty1())
        {
            cout << "Queue 1 Underflow!\n";
            return;
        }
        cout << arr[front1] << " deleted from Queue 1.\n";
        front1++;
    }

    void dequeue2()
    {
        if (isEmpty2())
        {
            cout << "Queue 2 Underflow!\n";
            return;
        }
        cout << arr[front2] << " deleted from Queue 2.\n";
        front2++;
    }

    void display1()
    {
        if (isEmpty1())
        {
            cout << "Queue 1 is empty.\n";
            return;
        }
        cout << "Queue 1: ";
        for (int i = front1; i <= rear1; ++i)
            cout << arr[i] << " ";
        cout << '\n';
    }

    void display2()
    {
        if (isEmpty2())
        {
            cout << "Queue 2 is empty.\n";
            return;
        }
        cout << "Queue 2: ";
        for (int i = front2; i <= rear2; ++i)
            cout << arr[i] << " ";
        cout << '\n';
    }

    ~TwoQueues() { delete[] arr; }
};

int main()
{
    int n, ch, value, qType;
    cout << "Enter total array size: ";
    cin >> n;

    if (n < 2)
    {
        cout << "Size too small. Need at least 2.\n";
        return 0;
    }

    TwoQueues tq(n);

    while (true)
    {
        cout << "\n--- Multiple Queue Operations ---\n";
        cout << "1. Add to Queue\n";
        cout << "2. Delete from Queue\n";
        cout << "3. Display Queue\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> ch;

        switch (ch)
        {
        case 1:
            cout << "Enter queue number (1 or 2): ";
            cin >> qType;
            cout << "Enter value: ";
            cin >> value;
            if (qType == 1)
                tq.enqueue1(value);
            else if (qType == 2)
                tq.enqueue2(value);
            else
                cout << "Invalid queue number!\n";
            break;

        case 2:
            cout << "Enter queue number (1 or 2): ";
            cin >> qType;
            if (qType == 1)
                tq.dequeue1();
            else if (qType == 2)
                tq.dequeue2();
            else
                cout << "Invalid queue number!\n";
            break;

        case 3:
            tq.display1();
            tq.display2();
            break;

        case 4:
            return 0;

        default:
            cout << "Invalid option!\n";
        }
    }
}

```
## **Output**

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS29.exe'
Enter total array size: 5
--- Multiple Queue Operations ---
1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter your choice: 1
Enter queue number (1 or 2): 1
Enter value: 10
10 inserted in Queue 1.

--- Multiple Queue Operations ---
1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter your choice: 1
Enter queue number (1 or 2): 2
Enter value: 20
20 inserted in Queue 2.

--- Multiple Queue Operations ---
1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter your choice: 1
Enter queue number (1 or 2): 2
Enter value: 30
30 inserted in Queue 2.

--- Multiple Queue Operations ---
1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter your choice: 3
Queue 1: 10
Queue 2: 20 30

--- Multiple Queue Operations ---
1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter your choice: 4

## **Conclusion**

The multiple queue implementation using a single array demonstrates efficient memory management and resource sharing between independent queues operating on FIFO principles with O(1) time complexity for add and delete operations. 