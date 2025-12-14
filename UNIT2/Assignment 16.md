# Assignment 16

## Title  
**Write a C++ program to implement a Set using a Generalized Linked List (GLL).**  
_For example:_  
Let **S = { p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1} }_  
Store this structure using a Generalized Linked List and display the elements in correct set notation format._

---

## Theory  

A **Generalized Linked List (GLL)** is a type of linked list where each node can store either:
1. A **data element** (called an atom), or  
2. A **pointer to another list**, forming a hierarchical or nested structure.

This makes GLLs suitable for representing **sets within sets**, **mathematical expressions**, and **hierarchical data**.

---
### Structure of a GLL Node:
Each node in a GLL generally has two fields:
- **flag** → identifies whether the node stores data (`0`) or a sublist (`1`).  
- **ptr** → pointer to the next node in the same list.  
- **down** → pointer to the sublist if the flag is `1`.

---
### Applications:
- Representation of **nested sets** (e.g., `{a, {b, c}, d}`)  
- Representation of **mathematical expressions**  
- Used in **compiler design**, **AI data structures**, and **symbolic computations**

---

## Algorithm  

**Algorithm to Create and Display a Generalized Linked List**

### Step 1: Start  
### Step 2: Create a Node with fields  
   - `flag`, `data`, `down`, and `next`.  
### Step 3: If the input element is an atom, store it in the `data` field and set `flag = 0`.  
### Step 4: If the input element is a sublist, recursively create its list and set `flag = 1`.  
### Step 5: Link the nodes appropriately using `next` pointers.  
### Step 6: For displaying,  
   - Print `{` when entering a sublist.  
   - Print elements recursively separated by commas.  
   - Print `}` when exiting the sublist.  
### Step 7: Stop.
---

## Code 

```
#include <iostream>
#include <string>
using namespace std;

struct GNode
{
    int flag;    
    string data; 
    GNode *next; 
    GNode *down; 

    GNode(int f = 0, string d = "")
    {
        flag = f;
        data = d;
        next = down = nullptr;          
    }
};


void display(GNode *head)
{
    cout << "{";
    GNode *temp = head;

    while (temp)
    {
        if (temp->flag == 0)
            cout << temp->data; 
        else
            display(temp->down); 

        if (temp->next)
            cout << ", ";
        temp = temp->next;
    }

    cout << "}";
}

int main()
{
    
    GNode *yz = new GNode(1);
    yz->down = new GNode(0, "y");
    yz->down->next = new GNode(0, "z");

    GNode *uv = new GNode(1);
    uv->down = new GNode(0, "u");
    uv->down->next = new GNode(0, "v");

    GNode *emptySet = new GNode(1);
    emptySet->down = nullptr;

   
    GNode *inner = new GNode(0, "r");
    inner->next = new GNode(0, "s");
    inner->next->next = new GNode(0, "t");
    inner->next->next->next = emptySet; 
    emptySet->next = uv;                
    uv->next = new GNode(0, "w");
    uv->next->next = new GNode(0, "x");
    uv->next->next->next = yz; 
    yz->next = new GNode(0, "a1");
    yz->next->next = new GNode(0, "b1");

    GNode *innerList = new GNode(1); 
    innerList->down = inner;

    
    GNode *S = new GNode(0, "p");
    S->next = new GNode(0, "q");
    S->next->next = innerList;

    cout << "Set S = ";
    display(S);
    cout << endl;

    return 0;
}

```
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS16.exe'
Set S = {p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1}}

## Conclusion
This program successfully represents a Set using a Generalized Linked List (GLL), allowing the storage and display of nested sets in proper set notation. It demonstrates the recursive structure and flexibility of GLLs for handling hierarchical data efficiently.
