# Assignment 15

## Title  
**Write a C++ program to store a binary number using a doubly linked list. Implement the following functions:  
a) Calculate and display the 1’s complement and 2’s complement of the stored binary number.  
b) Perform addition of two binary numbers represented using doubly linked lists and display the result.**

---

## Theory  

A **Doubly Linked List (DLL)** is a data structure where each node contains:  
1. **Data field** — stores the binary digit (0 or 1).  
2. **Pointer fields** — `prev` points to the previous node and `next` points to the next node.  

**Operations Implemented:**  

1. **1’s Complement:**  
   Flips each bit of the binary number (0 → 1, 1 → 0).  

2. **2’s Complement:**  
   Obtained by adding 1 to the 1’s complement of the binary number.  

3. **Binary Addition:**  
   Adds two binary numbers represented as doubly linked lists.  
   - Traverses both lists from the least significant bit (tail) to the most significant bit (head).  
   - Carries are handled appropriately to produce the correct sum.  

**Advantages of Using DLL for Binary Numbers:**  
- Easy traversal in both directions (forward and backward).  
- Efficient insertion or deletion of bits at any position.  
- Suitable for arithmetic operations like addition and complement calculations.  

## Code
```
#include <iostream>
using namespace std;

struct Node
{
    int bit;
    Node *prev;
    Node *next;
};

class DoublyLinkedList
{
public:
    Node *head, *tail;

    DoublyLinkedList()
    {
        head = tail = nullptr;
    }

    void insert(int b)
    {
        Node *newNode = new Node;
        newNode->bit = b;
        newNode->prev = newNode->next = nullptr;

        if (!head)
            head = tail = newNode;
        else
        {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
    }

    void display()
    {
        Node *temp = head;
        while (temp)
        {
            cout << temp->bit;
            temp = temp->next;
        }
    }

    
    DoublyLinkedList onesComplement()
    {
        DoublyLinkedList result;
        Node *temp = head;
        while (temp)
        {
            result.insert(temp->bit == 0 ? 1 : 0);
            temp = temp->next;
        }
        return result;
    }

    
    DoublyLinkedList twosComplement()
    {
        DoublyLinkedList result = onesComplement();
        Node *temp = result.tail;
        int carry = 1;
        while (temp)
        {
            int sum = temp->bit + carry;
            temp->bit = sum % 2;
            carry = sum / 2;
            temp = temp->prev;
        }
        if (carry == 1)
            result.insertFront(1);
        return result;
    }

    
    void insertFront(int b)
    {
        Node *newNode = new Node;
        newNode->bit = b;
        newNode->prev = nullptr;
        newNode->next = head;
        if (head)
            head->prev = newNode;
        else
            tail = newNode;
        head = newNode;
    }

    
    static DoublyLinkedList add(DoublyLinkedList &A, DoublyLinkedList &B)
    {
        DoublyLinkedList result;
        Node *p = A.tail;
        Node *q = B.tail;
        int carry = 0;

        while (p != nullptr || q != nullptr || carry)
        {
            int sum = carry;
            if (p)
            {
                sum += p->bit;
                p = p->prev;
            }
            if (q)
            {
                sum += q->bit;
                q = q->prev;
            }

            result.insertFront(sum % 2);
            carry = sum / 2;
        }
        return result;
    }
};

int main()
{
    DoublyLinkedList num1, num2;

    int n1, n2;
    cout << "Enter number of bits in first binary number: ";
    cin >> n1;
    cout << "Enter bits: ";
    for (int i = 0; i < n1; i++)
    {
        int b;
        cin >> b;
        num1.insert(b);
    }

    cout << "Enter number of bits in second binary number: ";
    cin >> n2;
    cout << "Enter bits: ";
    for (int i = 0; i < n2; i++)
    {
        int b;
        cin >> b;
        num2.insert(b);
    }

    cout << "\nBinary Number 1: ";
    num1.display();

    cout << "\nBinary Number 2: ";
    num2.display();

    
    DoublyLinkedList oneComp = num1.onesComplement();
    cout << "\n\n1's Complement of Number 1: ";
    oneComp.display();

    
    DoublyLinkedList twoComp = num1.twosComplement();
    cout << "\n2's Complement of Number 1: ";
    twoComp.display();

    DoublyLinkedList sum = DoublyLinkedList::add(num1, num2);
    cout << "\n\nAddition of Number 1 and Number 2: ";
    sum.display();

    cout << endl;
    return 0;
}

```
## Output 

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS15.exe'
Enter number of bits in first binary number: 4
Enter bits: 1 0 0 1
Enter number of bits in second binary number: 4
Enter bits: 1 0 0 0

Binary Number 1: 1001
Binary Number 2: 1000

1's Complement of Number 1: 0110
2's Complement of Number 1: 0111

Addition of Number 1 and Number 2: 10001

## Conclusion  

The program effectively uses a **Doubly Linked List** to store and manipulate binary numbers. It demonstrates how to calculate **1’s and 2’s complements** and perform **binary addition** dynamically. Using DLLs allows flexible and efficient operations on binary numbers, showcasing the practical use of pointers and dynamic memory management in C++.
