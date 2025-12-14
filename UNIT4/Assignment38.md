# Assignment 38

##Write a program to efficiently search a particular employee record by using Tree data structure. Also sort the data on emp­id in ascending order.

## Theory

This program uses a Binary Search Tree (BST) to manage employee records efficiently. Each node in the BST represents an employee with details like employee ID (empid), name, and salary. The BST is organized such that employee IDs are used as keys, allowing for efficient insertion, search, and sorting operations.

Key operations implemented:
- **Insert**: Adds a new employee record to the BST, maintaining the BST property (left subtree < root < right subtree).
- **Search**: Quickly finds an employee record by ID using the BST search algorithm.
- **Inorder Traversal**: Displays all employee records sorted by employee ID in ascending order, as inorder traversal of a BST visits nodes in sorted order.

The BST ensures that search operations are performed in O(log n) time on average, making it efficient for large datasets. The inorder traversal naturally sorts the data by empid without additional sorting algorithms.

## Code

```cpp
#include <iostream>
#include <string>
using namespace std;

// Employee structure
struct Employee
{
    int empId;
    string name;
    string department;
    double salary;

    Employee(int id, string n, string dept, double sal) : empId(id), name(n), department(dept), salary(sal) {}
};

// Node structure for BST
struct Node
{
    Employee data;
    Node *left;
    Node *right;

    Node(Employee emp) : data(emp), left(nullptr), right(nullptr) {}
};

// BST class for Employee records
class EmployeeBST
{
private:
    Node *root;

    // Helper function to insert an employee (sorted by empId ascending)
    Node *insertHelper(Node *node, Employee emp)
    {
        if (node == nullptr)
        {
            return new Node(emp); // Dynamic allocation
        }
        if (emp.empId < node->data.empId)
        {
            node->left = insertHelper(node->left, emp);
        }
        else if (emp.empId > node->data.empId)
        {
            node->right = insertHelper(node->right, emp);
        } // Ignore duplicates
        return node;
    }

    // Helper function to search for an employee by empId
    Employee *searchHelper(Node *node, int empId)
    {
        if (node == nullptr || node->data.empId == empId)
        {
            return (node != nullptr) ? &node->data : nullptr;
        }
        if (empId < node->data.empId)
        {
            return searchHelper(node->left, empId);
        }
        else
        {
            return searchHelper(node->right, empId);
        }
    }

    // Helper function for inorder display (sorted by empId)
    void displaySortedHelper(Node *node)
    {
        if (node == nullptr)
        {
            return;
        }
        displaySortedHelper(node->left);
        cout << "EmpID: " << node->data.empId << ", Name: " << node->data.name
             << ", Department: " << node->data.department << ", Salary: " << node->data.salary << endl;
        displaySortedHelper(node->right);
    }

public:
    EmployeeBST() : root(nullptr) {}

    ~EmployeeBST()
    {
        // Simplified: In real code, implement a function to delete all nodes
    }

    // Insert an employee
    void insert(Employee emp)
    {
        root = insertHelper(root, emp);
    }

    // Search for an employee by empId
    Employee *search(int empId)
    {
        return searchHelper(root, empId);
    }

    // Display all employees sorted by empId (inorder traversal)
    void displaySorted()
    {
        if (root == nullptr)
        {
            cout << "No employee records." << endl;
            return;
        }
        cout << "Employee Records (Sorted by EmpID):" << endl;
        displaySortedHelper(root);
    }
};

int main()
{
    EmployeeBST bst;
    int choice;

    do
    {
        cout << "\nEmployee Record Management Menu:" << endl;
        cout << "1. Add Employee" << endl;
        cout << "2. Search Employee by EmpID" << endl;
        cout << "3. Display All Employees (Sorted by EmpID)" << endl;
        cout << "4. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
        {
            int id;
            string name, dept;
            double sal;
            cout << "Enter EmpID: ";
            cin >> id;
            cout << "Enter Name: ";
            cin.ignore(); // To handle newline
            getline(cin, name);
            cout << "Enter Department: ";
            getline(cin, dept);
            cout << "Enter Salary: ";
            cin >> sal;
            bst.insert(Employee(id, name, dept, sal));
            cout << "Employee added." << endl;
            break;
        }
        case 2:
        {
            int id;
            cout << "Enter EmpID to search: ";
            cin >> id;
            Employee *emp = bst.search(id);
            if (emp)
            {
                cout << "Found: EmpID: " << emp->empId << ", Name: " << emp->name
                     << ", Department: " << emp->department << ", Salary: " << emp->salary << endl;
            }
            else
            {
                cout << "Employee with EmpID " << id << " not found." << endl;
            }
            break;
        }
        case 3:
            bst.displaySorted();
            break;
        case 4:
            cout << "Exiting..." << endl;
            break;
        default:
            cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 4);

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS38.exe'
Employee Record Management Menu:
1. Add Employee
2. Search Employee by EmpID
3. Display All Employees (Sorted by EmpID)
4. Exit
Enter your choice: 1
Enter EmpID: 101
Enter Name: Kanchan
Enter Department: Cs
Enter Salary: 50000
Employee added.

Employee Record Management Menu:
1. Add Employee
2. Search Employee by EmpID
3. Display All Employees (Sorted by EmpID)
4. Exit
Enter your choice: 1
Enter EmpID: 100
Enter Name: Kapil
Enter Department: ENTC
Enter Salary: 70000
Employee added.

Employee Record Management Menu:
1. Add Employee
2. Search Employee by EmpID
3. Display All Employees (Sorted by EmpID)
4. Exit
Enter your choice: 3
Employee Records (Sorted by EmpID):
EmpID: 100, Name: Kapil, Department: ENTC, Salary: 70000
EmpID: 101, Name: Kanchan, Department: Cs, Salary: 50000

Employee Record Management Menu:
1. Add Employee
2. Search Employee by EmpID
3. Display All Employees (Sorted by EmpID)
4. Exit
Enter your choice: 2
Enter EmpID to search: 100
Found: EmpID: 100, Name: Kapil, Department: ENTC, Salary: 70000

Employee Record Management Menu:
1. Add Employee
2. Search Employee by EmpID
3. Display All Employees (Sorted by EmpID)
4. Exit
Enter your choice: 4
Exiting...

```

## Conclusion

This program demonstrates the practical application of Binary Search Trees for managing employee records. By using employee ID as the key, the BST enables efficient insertion and search operations, typically in O(log n) time. The inorder traversal provides a natural way to display records sorted by ID without requiring additional sorting logic. This approach is particularly useful for database-like applications where quick lookups and sorted data retrieval are essential.
