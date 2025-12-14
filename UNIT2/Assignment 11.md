# Assignment 11

## Title  
### **Implementation of Singly Linked List to Manage ‘Vertex Club’ Membership Records**  

The Department of Computer Engineering has a student club named **‘Vertex Club’** for second, third, and final year students. In this club, the **first member is the President** and the **last member is the Secretary**. The program allows dynamic management of the club membership by enabling the user to **add or delete members**, **count total members**, **display the list of members**, and **concatenate two division lists**. Additional operations include **reversing the list**, **searching for a member by PRN**, and **sorting the list by PRN**, making it a comprehensive tool to manage the club efficiently.

## Theory  

A **Linked List** is a dynamic data structure where elements, called **nodes**, are connected using pointers. Unlike arrays, linked lists do not require contiguous memory, making them suitable for applications where the number of elements may vary over time.

Each node contains two fields:  
1. **Data Field** — stores information; in this program, the **PRN** and **Name** of a club member.  
2. **Pointer Field** — stores the address of the next node in the list.

### Application in Vertex Club
In the context of the **Vertex Club** (a student club in the Computer Engineering department):  
- The **first node** represents the **President**.  
- The **last node** represents the **Secretary**.  
- **Intermediate nodes** represent general **Members**.  

This structure allows dynamic management of membership records, including:  
- **Adding members** at any position (President, Secretary, or in-between).  
- **Deleting members** efficiently without shifting other elements.  
- **Counting members** to track club size dynamically.  
- **Displaying members** in order of joining.  
- **Concatenating lists** for merging members from different divisions.  
- **Reversing the list** to change display order.  
- **Searching by PRN** to find specific members quickly.  
- **Sorting by PRN** for ordered record management.

### Advantages of Using Linked List
- **Dynamic Size:** Memory is allocated as needed; no wastage.  
- **Efficient Insert/Delete:** Adding or removing members does not require shifting elements as in arrays.  
- **Flexibility:** Easy to implement additional operations like reverse, search, and concatenate.  
- **Practical Analogy:** Club membership changes frequently; a linked list reflects real-life addition/removal of members naturally.

This program demonstrates **data structures and algorithms concepts** applied to a real-world scenario, reinforcing understanding of **pointers, dynamic memory allocation, and linked list manipulations** in C++.

## Code
```
#include <iostream>
#include <string>

using namespace std;

struct Node {
    int prn;
    string name;
    Node* next;

    Node(int prn, string name) {
        this->prn = prn;
        this->name = name;
        this->next = nullptr;
    }
};

class VertexClub {
    Node* head;

public:
    VertexClub() {
        head = nullptr;
    }

    void addPresident(int prn, string name) {
        Node* newNode = new Node(prn, name);
        newNode->next = head;
        head = newNode;
    }

    void addSecretary(int prn, string name) {
        Node* newNode = new Node(prn, name);
        if (head == nullptr) {
            head = newNode;
        } else {
            Node* temp = head;
            while (temp->next != nullptr)
                temp = temp->next;
            temp->next = newNode;
        }
    }

    void addMember(int afterPRN, int prn, string name) {
        Node* temp = head;
        while (temp != nullptr && temp->prn != afterPRN)
            temp = temp->next;

        if (temp == nullptr) {
            cout << "Member with PRN " << afterPRN << " not found.\n";
            return;
        }

        Node* newNode = new Node(prn, name);
        newNode->next = temp->next;
        temp->next = newNode;
    }

    // Delete President
    void deletePresident() {
        if (head == nullptr) {
            cout << "List is empty.\n";
            return;
        }
        Node* temp = head;
        head = head->next;
        delete temp;
    }

    // Delete Secretary
    void deleteSecretary() {
        if (head == nullptr) {
            cout << "List is empty.\n";
            return;
        }
        if (head->next == nullptr) {
            delete head;
            head = nullptr;
            return;
        }
        Node* temp = head;
        while (temp->next->next != nullptr)
            temp = temp->next;
        delete temp->next;
        temp->next = nullptr;
    }

    // Delete Member by PRN
    void deleteMember(int prn) {
        if (head == nullptr) {
            cout << "List is empty.\n";
            return;
        }

        if (head->prn == prn) {
            deletePresident();
            return;
        }

        Node* temp = head;
        while (temp->next != nullptr && temp->next->prn != prn)
            temp = temp->next;

        if (temp->next == nullptr) {
            cout << "Member not found.\n";
            return;
        }

        Node* del = temp->next;
        temp->next = del->next;
        delete del;
    }

    // Display Members
    void display() {
        if (head == nullptr) {
            cout << "No members in the list.\n";
            return;
        }

        Node* temp = head;
        cout << "Vertex Club Members:\n";
        while (temp != nullptr) {
            cout << "PRN: " << temp->prn << ", Name: " << temp->name << endl;
            temp = temp->next;
        }
    }

    // Count Members
    int count() {
        int c = 0;
        Node* temp = head;
        while (temp != nullptr) {
            c++;
            temp = temp->next;
        }
        return c;
    }

    // Search by PRN
    void search(int prn) {
        Node* temp = head;
        while (temp != nullptr) {
            if (temp->prn == prn) {
                cout << "Member found: " << temp->name << " (PRN: " << temp->prn << ")\n";
                return;
            }
            temp = temp->next;
        }
        cout << "Member with PRN " << prn << " not found.\n";
    }

    // Reverse the list
    void reverse() {
        Node* prev = nullptr;
        Node* current = head;
        Node* next = nullptr;

        while (current != nullptr) {
            next = current->next;
            current->next = prev;
            prev = current;
            current = next;
        }
        head = prev;
    }

    
    void sortByPRN() {
        if (!head || !head->next) return;

        for (Node* i = head; i->next != nullptr; i = i->next) {
            for (Node* j = head; j->next != nullptr; j = j->next) {
                if (j->prn > j->next->prn) {
                    swap(j->prn, j->next->prn);
                    swap(j->name, j->next->name);
                }
            }
        }
    }

    void concatenate(VertexClub& other) {
        if (head == nullptr) {
            head = other.head;
            return;
        }
        Node* temp = head;
        while (temp->next != nullptr)
            temp = temp->next;
        temp->next = other.head;
    }

    
    ~VertexClub() {
        Node* current = head;
        while (current != nullptr) {
            Node* next = current->next;
            delete current;
            current = next;
        }
    }
};


int main() {
    VertexClub clubA, clubB;
    int choice, prn, afterPRN;
    string name;

    do {
        cout << "\n----- Vertex Club Menu -----\n";
        cout << "1. Add President\n2. Add Secretary\n3. Add Member\n";
        cout << "4. Delete President\n5. Delete Secretary\n6. Delete Member\n";
        cout << "7. Display Members\n8. Count Members\n9. Search by PRN\n";
        cout << "10. Reverse List\n11. Sort by PRN\n12. Concatenate Another List\n";
        cout << "13. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter PRN and Name for President: ";
                cin >> prn >> name;
                clubA.addPresident(prn, name);
                break;

            case 2:
                cout << "Enter PRN and Name for Secretary: ";
                cin >> prn >> name;
                clubA.addSecretary(prn, name);
                break;

            case 3:
                cout << "Enter PRN after which to add member: ";
                cin >> afterPRN;
                cout << "Enter PRN and Name for new member: ";
                cin >> prn >> name;
                clubA.addMember(afterPRN, prn, name);
                break;

            case 4:
                clubA.deletePresident();
                break;

            case 5:
                clubA.deleteSecretary();
                break;

            case 6:
                cout << "Enter PRN of member to delete: ";
                cin >> prn;
                clubA.deleteMember(prn);
                break;

            case 7:
                clubA.display();
                break;

            case 8:
                cout << "Total members: " << clubA.count() << endl;
                break;

            case 9:
                cout << "Enter PRN to search: ";
                cin >> prn;
                clubA.search(prn);
                break;

            case 10:
                clubA.reverse();
                cout << "List reversed.\n";
                break;

            case 11:
                clubA.sortByPRN();
                cout << "List sorted by PRN.\n";
                break;

            case 12:
                cout << "Creating second list to concatenate.\n";
                clubB.addPresident(201, "Alice");
                clubB.addSecretary(205, "Bob");
                clubB.addMember(201, 202, "Charlie");
                cout << "Second list:\n";
                clubB.display();

                clubA.concatenate(clubB);
                cout << "Lists concatenated.\n";
                break;

            case 13:
                cout << "Exiting...\n";
                break;

            default:
                cout << "Invalid choice!\n";
        }

    } while (choice != 13);

    return 0;
}

```
## Output

----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 1
Enter PRN and Name for President: 100 John
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 2
Enter PRN and Name for Secretary: 200 Jane
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 3
Enter PRN after which to add member: 100
Enter PRN and Name for new member: 150 Bob
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 7
Vertex Club Members:
PRN: 100, Name: John
PRN: 150, Name: Bob
PRN: 200, Name: Jane
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 8
Total members: 3
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 9
Enter PRN to search: 150
Member found: Bob (PRN: 150)
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 10
List reversed.
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 7
Vertex Club Members:
PRN: 200, Name: Jane
PRN: 150, Name: Bob
PRN: 100, Name: John
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 11
List sorted by PRN.
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 7
Vertex Club Members:
PRN: 100, Name: John
PRN: 150, Name: Bob
PRN: 200, Name: Jane
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 12
Creating second list to concatenate.
Second list:
Vertex Club Members:
PRN: 201, Name: Alice
PRN: 202, Name: Charlie
PRN: 205, Name: Bob
Lists concatenated.
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 7
Vertex Club Members:
PRN: 100, Name: John
PRN: 150, Name: Bob
PRN: 200, Name: Jane
PRN: 201, Name: Alice
PRN: 202, Name: Charlie
PRN: 205, Name: Bob
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 6
Enter PRN of member to delete: 150
----- Vertex Club Menu -----
1. Add President
2. Add Secretary
3. Add Member
4. Delete President
5. Delete Secretary
6. Delete Member
7. Display Members
8. Count Members
9. Search by PRN
10. Reverse List
11. Sort by PRN
12. Concatenate Another List
13. Exit
Enter choice: 13
Exiting..

## Conclusion  

The program successfully implements a **Singly Linked List** to manage *Vertex Club* membership records. It demonstrates dynamic management of student records, allowing addition and deletion of members (including President and Secretary) efficiently. Additional operations like **counting members, displaying the list, concatenation of lists, reversing the list, searching by PRN, and sorting by PRN** provide comprehensive control over membership data.  


