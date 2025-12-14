# **Assignment 27**

## **Title**

Pizza parlour accepting maximum n orders. Orders are served on an FCFS basis. Order once placed can't be cancelled. Write C++ program to simulate the system using circular QUEUE.

## **Theory**

A circular queue is a linear data structure that follows the First-Come-First-Served (FCFS) principle with efficient space utilization by connecting the end of the queue back to the beginning. This makes it ideal for systems with a fixed capacity like a pizza parlour that can handle a maximum number of orders at once.

**Key Concepts:**

1. **Circular Queue Structure**:
   - Fixed-size array implementation
   - Front pointer indicates the first order position
   - Rear pointer indicates the last order position
   - When rear reaches the end, it wraps around to the beginning if space is available

2. **Queue States**:
   - **Empty**: front == -1
   - **Full**: (rear + 1) % size == front
   - **Normal**: Orders between front and rear (circularly)

3. **FCFS (First-Come-First-Served)**:
   - Orders are placed at the rear
   - Orders are served from the front
   - No cancellation allowed once placed
   - Maintains strict order of service

4. **Circular Behavior**:
   - Uses modulo arithmetic: (index + 1) % size
   - Prevents wasted space in the array
   - Allows reuse of freed positions at the front

**Algorithm Steps:**

**Place Order (Enqueue):**
1. Check if queue is full using: (rear + 1) % size == front
2. If full, reject the order
3. If empty, set front = rear = 0
4. Otherwise, rear = (rear + 1) % size
5. Store order at rear position

**Serve Order (Dequeue):**
1. Check if queue is empty (front == -1)
2. If empty, display no orders message
3. Retrieve order at front position
4. If only one order, reset front = rear = -1
5. Otherwise, front = (front + 1) % size

**Display Orders:**
1. Check if queue is empty
2. Traverse circularly from front to rear
3. Display all pending orders

## **Code**

```cpp
#include <iostream>
using namespace std;

class CircularQueue {
    int front, rear, size;
    int *queue;

public:
    CircularQueue(int n) {
        size = n;
        queue = new int[size];
        front = rear = -1;
    }

    bool isFull() {
        return (front == (rear + 1) % size);
    }

    bool isEmpty() {
        return (front == -1);
    }

    void placeOrder(int orderID) {
        if (isFull()) {
            cout << "Order capacity is FULL. Cannot place more orders!\n";
            return;
        }

        if (isEmpty()) {
            front = rear = 0;
        } else {
            rear = (rear + 1) % size;
        }

        queue[rear] = orderID;
        cout << "Order " << orderID << " placed successfully.\n";
    }

    void serveOrder() {
        if (isEmpty()) {
            cout << "No orders to serve!\n";
            return;
        }

        cout << "Order " << queue[front] << " is served.\n";

        if (front == rear) { 
            front = rear = -1; 
        } else {
            front = (front + 1) % size;
        }
    }

    void displayOrders() {
        if (isEmpty()) {
            cout << "No orders in waiting.\n";
            return;
        }

        cout << "Orders in queue: ";
        int i = front;

        while (true) {
            cout << queue[i] << " ";
            if (i == rear)
                break;
            i = (i + 1) % size;
        }

        cout << endl;
    }
};

int main() {
    int n, choice, orderID;

    cout << "Enter maximum number of orders: ";
    cin >> n;

    CircularQueue pq(n);

    while (true) {
        cout << "\n---- Pizza Parlour System ----\n";
        cout << "1. Place Order\n";
        cout << "2. Serve Order\n";
        cout << "3. Display Pending Orders\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter Order ID: ";
            cin >> orderID;
            pq.placeOrder(orderID);
            break;

        case 2:
            pq.serveOrder();
            break;

        case 3:
            pq.displayOrders();
            break;

        case 4:
            cout << "Exiting System.\n";
            return 0;

        default:
            cout << "Invalid choice! Try again.\n";
        }
    }
}

```
## **Output**
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS27.exe'
Enter maximum number of orders: 2
---- Pizza Parlour System ----
1. Place Order
2. Serve Order
3. Display Pending Orders
4. Exit
Enter your choice: 1
Enter Order ID: 101
Order 101 placed successfully.

---- Pizza Parlour System ----
1. Place Order
2. Serve Order
3. Display Pending Orders
4. Exit
Enter your choice: 3
Orders in queue: 101

---- Pizza Parlour System ----
1. Place Order
2. Serve Order
3. Display Pending Orders
4. Exit
Enter your choice: 2
Order 101 is served.

---- Pizza Parlour System ----
1. Place Order
2. Serve Order
3. Display Pending Orders
4. Exit
Enter your choice: 3
No orders in waiting.

---- Pizza Parlour System ----
1. Place Order
2. Serve Order
3. Display Pending Orders
4. Exit
Enter your choice: 1
Enter Order ID: 102
Order 102 placed successfully.

---- Pizza Parlour System ----
1. Place Order
2. Serve Order
3. Display Pending Orders
4. Exit
Enter your choice: 2
Order 102 is served.

---- Pizza Parlour System ----
1. Place Order
2. Serve Order
3. Display Pending Orders
4. Exit
Enter your choice: 4
Exiting System.


## **Conclusion**

The circular queue implementation for the pizza parlour order management system demonstrates efficient space utilization and optimal performance for fixed-capacity service systems operating on FCFS principles. The circular nature eliminates the wasted space problem inherent in linear queues by reusing positions freed at the front when orders are served, achieving maximum capacity utilization with O(1) time complexity for both order placement and service operations. 