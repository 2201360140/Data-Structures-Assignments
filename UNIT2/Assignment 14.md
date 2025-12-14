# Assignment 14  

## Title  
**In the Second Year Computer Engineering class, there are two groups of students based on their favorite sports: 
●	Set A includes students who like Cricket.  
●	Set B includes students who like Football.  
Write a C++ program to represent these two sets using linked lists and perform the following operations:  
a) Find and display the set of students who like both Cricket and Football.  
b) Find and display the set of students who like either Cricket or Football, but not both.  
c) Display the number of students who like neither Cricket nor Football.**

---

## Theory  

A **Linked List** is a dynamic data structure where nodes are connected via pointers. Each node contains:  
- **Data field** — stores the student identifier or name.  
- **Pointer field** — points to the next node.  

**Set Operations Using Linked Lists:**  

1. **Intersection (A ∩ B):**  
   Finds students who like both Cricket and Football by comparing elements in both lists.  

2. **Exclusive Union (A ⊕ B):**  
   Finds students who like either Cricket or Football, but **not both**, by including only nodes present in one list.  

3. **Neither Set Count:**  
   Calculates the number of students who do not belong to either Set A or Set B, based on the total student population.  

**Advantages of Using Linked Lists for Sets:**  
- **Dynamic size:** Lists can grow or shrink as students are added or removed.  
- **Efficient insertion/deletion:** Elements can be added or removed without shifting memory as in arrays.  
- **Supports set operations:** Easy traversal and comparison for intersection, union, and complement operations.  

## Code

```
#include <iostream>
#include <string>
using namespace std;

struct Node
{
    string name;
    Node *next;
};

class LinkedList
{
public:
    Node *head;
    LinkedList() { head = nullptr; }

    void insert(string n)
    {
        Node *newNode = new Node;
        newNode->name = n;
        newNode->next = nullptr;

        // Insert at end
        if (!head)
        {
            head = newNode;
        }
        else
        {
            Node *temp = head;
            while (temp->next)
                temp = temp->next;
            temp->next = newNode;
        }
    }

    bool search(string n)
    {
        Node *temp = head;
        while (temp)
        {
            if (temp->name == n)
                return true;
            temp = temp->next;
        }
        return false;
    }

    void display()
    {
        Node *temp = head;
        if (!temp)
        {
            cout << "{ }";
            return;
        }
        cout << "{ ";
        while (temp)
        {
            cout << temp->name;
            if (temp->next)
                cout << ", ";
            temp = temp->next;
        }
        cout << " }";
    }
};


LinkedList intersection(LinkedList &A, LinkedList &B)
{
    LinkedList result;
    Node *temp = A.head;
    while (temp)
    {
        if (B.search(temp->name))
            result.insert(temp->name);
        temp = temp->next;
    }
    return result;
}

LinkedList symmetricDifference(LinkedList &A, LinkedList &B)
{
    LinkedList result;
    Node *temp = A.head;

    
    while (temp)
    {
        if (!B.search(temp->name))
            result.insert(temp->name);
        temp = temp->next;
    }

    
    temp = B.head;
    while (temp)
    {
        if (!A.search(temp->name))
            result.insert(temp->name);
        temp = temp->next;
    }

    return result;
}


int main()
{
    LinkedList cricket, football;
    int n, m, totalStudents;
    string name;

    cout << "Enter total number of students in class: ";
    cin >> totalStudents;

    cout << "\nEnter number of students who like Cricket: ";
    cin >> n;
    cout << "Enter names of students who like Cricket:\n";
    for (int i = 0; i < n; i++)
    {
        cin >> name;
        cricket.insert(name);
    }

    cout << "\nEnter number of students who like Football: ";
    cin >> m;
    cout << "Enter names of students who like Football:\n";
    for (int i = 0; i < m; i++)
    {
        cin >> name;
        football.insert(name);
    }

    cout << "\nSet A (Cricket): ";
    cricket.display();
    cout << "\nSet B (Football): ";
    football.display();

    
    LinkedList both = intersection(cricket, football);
    cout << "\n\n(a) Students who like BOTH Cricket and Football: ";
    both.display();

    
    LinkedList either = symmetricDifference(cricket, football);
    cout << "\n(b) Students who like EITHER Cricket or Football but NOT both: ";
    either.display();

    
    LinkedList unionList = symmetricDifference(cricket, football);
    Node *temp = intersection(cricket, football).head;
    while (temp)
    { 
        if (!unionList.search(temp->name))
            unionList.insert(temp->name);
        temp = temp->next;
    }

    int likeAtLeastOne = 0;
    Node *u = unionList.head;
    while (u)
    {
        likeAtLeastOne++;
        u = u->next;
    }

    int neither = totalStudents - likeAtLeastOne;
    cout << "\n(c) Number of students who like NEITHER Cricket nor Football: " << neither << endl;

    return 0;
}


```

##  Output
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS14.exe'
Enter total number of students in class: 8

Enter number of students who like Cricket: 3
Enter names of students who like Cricket:
kanchan 
pogo
nehu

Enter number of students who like Football: 2
Enter names of students who like Football:
Kanchan
Kajal

Set A (Cricket): { kanchan, pogo, nehu }
Set B (Football): { Kanchan, Kajal }

(a) Students who like BOTH Cricket and Football: { }
(b) Students who like EITHER Cricket or Football but NOT both: { kanchan, pogo, nehu, Kanchan, Kajal }
(c) Number of students who like NEITHER Cricket nor Football: 3
## Conclusion  

The program effectively uses **linked lists to represent student groups** based on their favorite sports. It can **find students in both sets, students in only one set, and those in neither**, demonstrating practical applications of **dynamic data structures for set operations**. This approach reflects how linked lists can efficiently manage and manipulate group data in C++.