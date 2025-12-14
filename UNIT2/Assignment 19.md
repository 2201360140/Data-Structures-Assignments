
# Assignment 19

## Title  
**Write a C++ program to create a Doubly Linked List and perform the following operations:**  
1. **Insertion** (at beginning, at end, at a given position)  
2. **Deletion** (from beginning, from end, from a given position)  

---

## Theory  

A **Doubly Linked List (DLL)** is a dynamic data structure consisting of nodes where each node contains:  
1. **Data field** — stores the value.  
2. **Pointer fields** — `prev` points to the previous node, `next` points to the next node.  

**Operations on DLL:**  

**Insertion:**  
- **At beginning:** New node’s `next` points to current head; head’s `prev` updated; head updated.  
- **At end:** Traverse to last node; last node’s `next` points to new node; new node’s `prev` points to last node.  
- **At given position:** Traverse to the node before position; adjust `next` and `prev` pointers to insert new node.  

**Deletion:**  
- **From beginning:** Update head to head->next; set head->prev to nullptr; delete old head.  
- **From end:** Traverse to last node; last node’s `prev`’s `next` set to nullptr; delete last node.  
- **From given position:** Traverse to node at position; update `prev` and `next` of adjacent nodes; delete the node.  

**Advantages of DLL:**  
- Bidirectional traversal (forward & backward).  
- Efficient insertion/deletion anywhere in the list.  
- Dynamic size, no need to predefine list size.  

**Algorithm:**  
1. Start with an empty DLL.  
2. For **insertion**, check position and update pointers accordingly.  
3. For **deletion**, locate the node and adjust surrounding node pointers.  
4. Repeat operations as needed.  
5. Display list after every operation.  

---
## Code
```
#include <iostream>
using namespace std;


struct Node {
    int data;
    Node* prev;
    Node* next;
};

Node* createNode(int data) {
    Node* newNode = new Node();
    newNode->data = data;
    newNode->prev = nullptr;
    newNode->next = nullptr;
    return newNode;
}


void display(Node* head) {
    Node* temp = head;
    cout << "List: ";
    while (temp != nullptr) {
        cout << temp->data << " ";
        temp = temp->next;
    }
    cout << endl;
}

void insertAtBeginning(Node** head, int data) {
    Node* newNode = createNode(data);
    newNode->next = *head;
    if (*head != nullptr)
        (*head)->prev = newNode;
    *head = newNode;
}


void insertAtEnd(Node** head, int data) {
    Node* newNode = createNode(data);
    if (*head == nullptr) {
        *head = newNode;
        return;
    }
    Node* temp = *head;
    while (temp->next != nullptr)
        temp = temp->next;
    temp->next = newNode;
    newNode->prev = temp;
}


void insertAfterPosition(Node** head, int pos, int data) {
    Node* newNode = createNode(data);
    Node* temp = *head;

    for (int i = 1; i < pos && temp != nullptr; i++)
        temp = temp->next;

    if (temp == nullptr) {
        cout << "Position not found!" << endl;
        delete newNode;
        return;
    }

    newNode->next = temp->next;
    newNode->prev = temp;
    if (temp->next != nullptr)
        temp->next->prev = newNode;
    temp->next = newNode;
}


void deleteFromBeginning(Node** head) {
    if (*head == nullptr) {
        cout << "List is empty!" << endl;
        return;
    }
    Node* temp = *head;
    *head = (*head)->next;
    if (*head != nullptr)
        (*head)->prev = nullptr;
    delete temp;
}


void deleteFromEnd(Node** head) {
    if (*head == nullptr) {
        cout << "List is empty!" << endl;
        return;
    }
    Node* temp = *head;
    while (temp->next != nullptr)
        temp = temp->next;

    if (temp->prev != nullptr)
        temp->prev->next = nullptr;
    else
        *head = nullptr;

    delete temp;
}


void deleteAtPosition(Node** head, int pos) {
    if (*head == nullptr) {
        cout << "List is empty!" << endl;
        return;
    }

    Node* temp = *head;

    for (int i = 1; i < pos && temp != nullptr; i++)
        temp = temp->next;

    if (temp == nullptr) {
        cout << "Position not found!" << endl;
        return;
    }

    if (temp->prev != nullptr)
        temp->prev->next = temp->next;
    else
        *head = temp->next;

    if (temp->next != nullptr)
        temp->next->prev = temp->prev;

    delete temp;
}


int main() {
    Node* head = nullptr;
    int choice, data, pos;

    do {
        cout << "\n--- Doubly Linked List Operations ---\n";
        cout << "1. Insert at Beginning\n";
        cout << "2. Insert at End\n";
        cout << "3. Insert After Position\n";
        cout << "4. Delete from Beginning\n";
        cout << "5. Delete from End\n";
        cout << "6. Delete from Position\n";
        cout << "7. Display List\n";
        cout << "8. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter data: ";
            cin >> data;
            insertAtBeginning(&head, data);
            break;
        case 2:
            cout << "Enter data: ";
            cin >> data;
            insertAtEnd(&head, data);
            break;
        case 3:
            cout << "Enter position and data: ";
            cin >> pos >> data;
            insertAfterPosition(&head, pos, data);
            break;
        case 4:
            deleteFromBeginning(&head);
            break;
        case 5:
            deleteFromEnd(&head);
            break;
        case 6:
            cout << "Enter position: ";
            cin >> pos;
            deleteAtPosition(&head, pos);
            break;
        case 7:
            display(head);
            break;
        case 8:
            cout << "Exiting program..." << endl;
            break;
        default:
            cout << "Invalid choice!" << endl;
        }
    } while (choice != 8);

    return 0;
}

```
## Output

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 1
Enter data: 10

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 2
Enter data: 50

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 2
Enter data: 20

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 4

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 5

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 7
List: 50

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 1
Enter data: 20

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 6
Enter position: 1

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 7
List: 50

--- Doubly Linked List Operations ---
1. Insert at Beginning
2. Insert at End
3. Insert After Position
4. Delete from Beginning
5. Delete from End
6. Delete from Position
7. Display List
8. Exit
Enter your choice: 8
Exiting program...


## Conclusion  

Doubly Linked Lists provide a flexible way to store and manage data.  
They allow efficient insertion and deletion at any position and bidirectional traversal, making them suitable for various applications like text editors, browsers’ history, and memory management.