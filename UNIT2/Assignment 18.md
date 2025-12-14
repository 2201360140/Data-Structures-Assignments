# Assignment 18

## Title  
**Write a C++ program to implement Bubble Sort using a Doubly Linked List.**

---

## Theory  

A **Doubly Linked List (DLL)** is a data structure where each node contains:  
1. **Data field** — stores the value of the node.  
2. **Pointer fields** — `prev` points to the previous node, `next` points to the next node.  

**Bubble Sort** is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order.  

**Algorithm for Bubble Sort on Doubly Linked List:**  
1. Start from the head of the DLL.  
2. Compare the current node’s data with the next node’s data.  
3. If the current node’s data is greater than the next node’s, swap the data.  
4. Move to the next node and repeat step 2 until the end of the list.  
5. Repeat steps 1–4 until no swaps are required (list is sorted).  

**Advantages of DLL in Bubble Sort:**  
- Can traverse both forwards and backwards.  
- Easy swapping of node data.  
- Dynamic size adjustment without shifting elements as in arrays.

---
## Code
```
#include <iostream>
using namespace std;


struct Node
{
    int data;
    Node *prev;
    Node *next;
};


Node *createNode(int data)
{
    Node *newNode = new Node();
    newNode->data = data;
    newNode->prev = nullptr;
    newNode->next = nullptr;
    return newNode;
}


void insertEnd(Node **head, int data)
{
    Node *newNode = createNode(data);
    if (*head == nullptr)
    {
        *head = newNode;
        return;
    }

    Node *temp = *head;
    while (temp->next != nullptr)
        temp = temp->next;

    temp->next = newNode;
    newNode->prev = temp;
}


void display(Node *head)
{
    Node *temp = head;
    while (temp != nullptr)
    {
        cout << temp->data;
        if (temp->next != nullptr)
            cout << " <-> ";
        temp = temp->next;
    }
    cout << endl;
}


void bubbleSort(Node *head)
{
    if (head == nullptr)
        return;

    bool swapped;
    Node *ptr1;
    Node *lptr = nullptr; 
    do
    {
        swapped = false;
        ptr1 = head;

        while (ptr1->next != lptr)
        {
            if (ptr1->data > ptr1->next->data)
            {
                
                int temp = ptr1->data;
                ptr1->data = ptr1->next->data;
                ptr1->next->data = temp;
                swapped = true;
            }
            ptr1 = ptr1->next;
        }
        lptr = ptr1; 
    } while (swapped);
}


int main()
{
    Node *head = nullptr;
    int n, data;

    cout << "Enter number of nodes: ";
    cin >> n;

    cout << "Enter " << n << " elements:\n";
    for (int i = 0; i < n; i++)
    {
        cin >> data;
        insertEnd(&head, data);
    }

    cout << "\nDoubly Linked List before sorting:\n";
    display(head);

    bubbleSort(head);

    cout << "\nDoubly Linked List after Bubble Sort:\n";
    display(head);

    return 0;
}


```
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS18.exe'
Enter number of nodes: 6
Enter 6 elements:
45 34 67 90 2 6

Doubly Linked List before sorting:
45 <-> 34 <-> 67 <-> 90 <-> 2 <-> 6

Doubly Linked List after Bubble Sort:
2 <-> 6 <-> 34 <-> 45 <-> 67 <-> 90

## Conclusion  

Bubble Sort using a Doubly Linked List efficiently sorts elements while taking advantage of bidirectional traversal. This approach demonstrates the flexibility of DLLs in implementing simple sorting algorithms without using extra memory.