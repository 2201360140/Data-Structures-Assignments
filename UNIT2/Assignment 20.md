# Assignment 20

## Title

Given a list, split it into two sublists — one for the front half, and one for the back half. If the number of elements is odd, the extra element should go in the front list. So FrontBackSplit() on the list {2, 3, 5, 7, 11} should yield the two lists {2, 3, 5} and {7, 11}. Getting this right for all the cases is harder than it looks. You should check your solution against a few cases (length = 2, length = 3, length=4) to make sure that the list gets split correctly near the shortlist boundary conditions. If it works right for length=4, it probably works right for length=1000. You will probably need special case code to deal with the (length <2) cases.

## Theory

The Front-Back Split problem involves dividing a singly linked list into two halves. The challenge lies in handling various edge cases and ensuring the split is performed correctly for lists of different lengths.

**Key Concepts:**

1. **Two-Pointer Technique (Tortoise and Hare)**: We use two pointers moving at different speeds:
   - `slow` pointer moves one step at a time
   - `fast` pointer moves two steps at a time
   - When `fast` reaches the end, `slow` will be at the midpoint

2. **Handling Odd/Even Lists**:
   - For odd-length lists (e.g., 5 elements): Front gets 3, Back gets 2
   - For even-length lists (e.g., 4 elements): Front gets 2, Back gets 2

3. **Edge Cases**:
   - Empty list (length = 0): Both halves are null
   - Single element (length = 1): Front gets the element, Back is null
   - Two elements (length = 2): Each half gets one element

**Algorithm Steps:**

1. Handle special cases (empty or single-element list)
2. Initialize `slow` at the head and `fast` at the second node
3. Move `fast` two steps and `slow` one step until `fast` reaches the end
4. Split the list at `slow`'s position by breaking the link
5. Assign front and back references appropriately

## **Code**

```cpp
#include <iostream>
using namespace std;


struct Node
{
    int data;
    Node *next;
};


Node *createNode(int data)
{
    Node *newNode = new Node();
    newNode->data = data;
    newNode->next = nullptr;
    return newNode;
}


void append(Node **head, int data)
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
}


void display(Node *head)
{
    Node *temp = head;
    while (temp != nullptr)
    {
        cout << temp->data;
        if (temp->next != nullptr)
            cout << " -> ";
        temp = temp->next;
    }
    cout << endl;
}


void FrontBackSplit(Node *source, Node **frontRef, Node **backRef)
{
    if (source == nullptr || source->next == nullptr)
    {
        
        *frontRef = source;
        *backRef = nullptr;
        return;
    }

    Node *slow = source;
    Node *fast = source->next;

    
    while (fast != nullptr)
    {
        fast = fast->next;
        if (fast != nullptr)
        {
            slow = slow->next;
            fast = fast->next;
        }
    }

   
    *frontRef = source;
    *backRef = slow->next;
    slow->next = nullptr; 
}


int main()
{
    Node *head = nullptr;
    Node *front = nullptr;
    Node *back = nullptr;

    int n, value;
    cout << "Enter number of elements: ";
    cin >> n;

    cout << "Enter list elements:\n";
    for (int i = 0; i < n; i++)
    {
        cin >> value;
        append(&head, value);
    }

    cout << "\nOriginal List: ";
    display(head);

    FrontBackSplit(head, &front, &back);

    cout << "Front half: ";
    display(front);
    cout << "Back half: ";
    display(back);

    return 0;
}

```
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS20.exe'
Enter number of elements: 5
Enter list elements:
10 20 30 40 50
Original List: 10 -> 20 -> 30 -> 40 -> 50
Front half: 10 -> 20 -> 30
Back half: 40 -> 50

## **Conclusion**

The Front-Back Split algorithm efficiently divides a linked list into two halves using the two-pointer technique with O(n) time complexity and O(1) space complexity. 