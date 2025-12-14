# **Assignment 21**

## **Title**

WAP to build a simple stock price tracker that keeps a history of daily stock prices entered by the user. To allow users to go back and view or remove the most recent price, implement a stack using a linked list to store these integer prices. Implement the following operations:

1. **record(price)** – Add a new stock price (an integer) to the stack.
2. **remove()** – Remove and return the most recent price (top of the stack).
3. **latest()** – Return the most recent stock price without removing it.
4. **isEmpty()** – Check if there are no prices recorded.

## **Theory**

A stack is a linear data structure that follows the Last-In-First-Out (LIFO) principle, making it ideal for tracking stock prices where the most recent entry needs to be accessed first. In this implementation, we use a singly linked list to create a dynamic stack that can grow and shrink as needed without any fixed size limitation.

**Key Concepts:**

1. **Stack Operations**:
   - **Push (record)**: Add a new element to the top of the stack
   - **Pop (remove)**: Remove and return the top element from the stack
   - **Peek (latest)**: View the top element without removing it
   - **isEmpty**: Check if the stack has any elements

2. **Linked List Implementation**:
   - Each node contains a stock price and a pointer to the next node
   - The top pointer always points to the most recently added node
   - New nodes are inserted at the beginning (top) for O(1) insertion
   - Deletion also occurs from the beginning for O(1) removal

3. **Advantages of Using Stack for Stock Tracking**:
   - LIFO behavior naturally matches the requirement to access recent prices first
   - Dynamic size allows unlimited price history
   - Efficient O(1) time complexity for all operations
   - Easy to implement undo functionality

**Algorithm Steps:**

**Record Operation:**
1. Create a new node with the given price
2. Set the new node's next pointer to the current top
3. Update top to point to the new node

**Remove Operation:**
1. Check if stack is empty
2. Store the top node's price
3. Move top pointer to the next node
4. Delete the old top node
5. Return the stored price

**Latest Operation:**
1. Check if stack is empty
2. Return the price at the top node without modification

**isEmpty Operation:**
1. Check if top pointer is nullptr
2. Return true if empty, false otherwise

## **Code**

```cpp
#include <iostream>
#include <cstdlib> 
using namespace std;

const int MAX = 50;
int a[MAX];
int top = -1;

void push();
void pop();
void isEmpty();
void isFull();
void display();

int main()
{
    int ch;
    do
    {
        cout << "\n1 - Push\n2 - Pop\n3 - isEmpty\n4 - isFull\n5 - Display\n6 - Exit\n";
        cout << "Enter your choice: ";
        cin >> ch;

        switch (ch)
        {
        case 1:
            push();
            break;
        case 2:
            pop();
            break;
        case 3:
            isEmpty();
            break;
        case 4:
            isFull();
            break;
        case 5:
            display();
            break;
        case 6:
            exit(0);
            break;
        default:
            cout << "Wrong choice!" << endl;
        }

    } while (ch != 6);

    return 0;
}

void push()
{
    if (top == MAX - 1)
    {
        cout << "Stack is Full (Overflow)" << endl;
    }
    else
    {
        int value;
        cout << "Enter element to push: ";
        cin >> value;
        top++;
        a[top] = value;
        cout << "Element " << value << " pushed to stack." << endl;
    }
}

void pop()
{
    if (top == -1)
    {
        cout << "Stack is Empty (Underflow)" << endl;
    }
    else
    {
        int value = a[top];
        top--;
        cout << "Element " << value << " popped from stack." << endl;
    }
}

void isEmpty()
{
    if (top == -1)
    {
        cout << "Stack is Empty" << endl;
    }
    else
    {
        cout << "Stack is NOT Empty" << endl;
    }
}

void isFull()
{
    if (top == MAX - 1)
    {
        cout << "Stack is Full" << endl;
    }
    else
    {
        cout << "Stack is NOT Full" << endl;
    }
}
void display()
{
    if (top == -1)
    {
        cout << "Stack is Empty" << endl;
    }
    else
    {
        cout << "Stack elements (top to bottom): ";
        for (int i = top; i >= 0; i--)
        {
            cout << a[i] << " ";
        }
        cout << endl;
    }
}

```
---
## ***Output**
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS21.exe'
1 - Push
2 - Pop
3 - isEmpty
4 - isFull
5 - Display
6 - Exit
Enter your choice: 1
Enter element to push: 10
Element 10 pushed to stack.
1 - Push
2 - Pop
3 - isEmpty
4 - isFull
5 - Display
6 - Exit
Enter your choice: 1
Enter element to push: 20
Element 20 pushed to stack.
1 - Push
2 - Pop
3 - isEmpty
4 - isFull
5 - Display
6 - Exit
Enter your choice: 5
Stack elements (top to bottom): 20 10
1 - Push
2 - Pop
3 - isEmpty
4 - isFull
5 - Display
6 - Exit
Enter your choice: 2
Element 20 popped from stack.
1 - Push
2 - Pop
3 - isEmpty
4 - isFull
5 - Display
6 - Exit
Enter your choice: 2
Element 10 popped from stack.
1 - Push
2 - Pop
3 - isEmpty
4 - isFull
5 - Display
6 - Exit
Enter your choice: 3
Stack is Empty
1 - Push
2 - Pop
3 - isEmpty
4 - isFull
5 - Display
6 - Exit
Enter your choice: 6

## **Conclusion**

The stock price tracker implementation successfully demonstrates the practical application of stack data structure using linked lists. This implementation provides an efficient solution for maintaining a history of stock prices with constant time complexity O(1) for all core operations including recording new prices, removing the most recent entry, and viewing the latest price without modification. 