# Assignment 17

## Title  
**Write a C++ program to perform addition of two polynomials using a singly linked list.**

---

## Theory  

A **Polynomial** is an algebraic expression consisting of terms with **coefficients** and **powers** of a variable.  
A **Singly Linked List** can be used to represent a polynomial, where each **node stores a term**:  
- `coeff` → coefficient of the term  
- `pow` → power of the variable  
- `next` → pointer to the next term  

**Polynomial Addition:**  
- Two polynomials are traversed simultaneously.  
- Terms with the **same power** are added by summing their coefficients.  
- Terms with **different powers** are directly linked to the result list.  
- The resulting polynomial is a new linked list representing the sum.  

**Advantages of Using Linked List for Polynomials:**  
- Dynamic memory allocation (no fixed size needed).  
- Easy insertion/deletion of terms.  
- Efficient handling of sparse polynomials.

---
## Code
```
#include <iostream>
using namespace std;


struct Node
{
    int coeff;
    int pow;
    Node *next;
};


Node *createNode(int coeff, int pow)
{
    Node *newNode = new Node();
    newNode->coeff = coeff;
    newNode->pow = pow;
    newNode->next = nullptr;
    return newNode;
}


void append(Node **head, int coeff, int pow)
{
    Node *newNode = createNode(coeff, pow);
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
    if (!head)
    {
        cout << "0";
        return;
    }
    while (head != nullptr)
    {
        cout << head->coeff << "x^" << head->pow;
        head = head->next;
        if (head != nullptr && head->coeff >= 0)
            cout << " + ";
    }
    cout << endl;
}


Node *addPolynomials(Node *poly1, Node *poly2)
{
    Node *result = nullptr;

    while (poly1 != nullptr && poly2 != nullptr)
    {
        if (poly1->pow > poly2->pow)
        {
            append(&result, poly1->coeff, poly1->pow);
            poly1 = poly1->next;
        }
        else if (poly1->pow < poly2->pow)
        {
            append(&result, poly2->coeff, poly2->pow);
            poly2 = poly2->next;
        }
        else
        {
            append(&result, poly1->coeff + poly2->coeff, poly1->pow);
            poly1 = poly1->next;
            poly2 = poly2->next;
        }
    }

    while (poly1 != nullptr)
    {
        append(&result, poly1->coeff, poly1->pow);
        poly1 = poly1->next;
    }

    while (poly2 != nullptr)
    {
        append(&result, poly2->coeff, poly2->pow);
        poly2 = poly2->next;
    }

    return result;
}

int main()
{
    Node *poly1 = nullptr, *poly2 = nullptr, *sum = nullptr;
    int n, coeff, pow;

    cout << "Enter number of terms in first polynomial: ";
    cin >> n;
    cout << "Enter terms in descending order of power (coeff power):\n";
    for (int i = 0; i < n; i++)
    {
        cin >> coeff >> pow;
        append(&poly1, coeff, pow);
    }

    cout << "\nEnter number of terms in second polynomial: ";
    cin >> n;
    cout << "Enter terms in descending order of power (coeff power):\n";
    for (int i = 0; i < n; i++)
    {
        cin >> coeff >> pow;
        append(&poly2, coeff, pow);
    }

    cout << "\nFirst Polynomial: ";
    display(poly1);
    cout << "Second Polynomial: ";
    display(poly2);

    sum = addPolynomials(poly1, poly2);

    cout << "\nSum of Polynomials: ";
    display(sum);

    return 0;
}

```
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS17.exe'
Enter number of terms in first polynomial: 3
Enter terms in descending order of power (coeff power):
2 3
4 2
1 1

Enter number of terms in second polynomial: 3
Enter terms in descending order of power (coeff power):
2 3
2 2
1 1

First Polynomial: 2x^3 + 4x^2 + 1x^1
Second Polynomial: 2x^3 + 2x^2 + 1x^1

Sum of Polynomials: 4x^3 + 6x^2 + 2x^1

## Conclusion  

This program demonstrates how to represent polynomials using a singly linked list and perform addition efficiently.  
It highlights the flexibility of linked lists in handling dynamic algebraic expressions and combining terms with the same power.
