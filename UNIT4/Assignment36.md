# Assignment 36

## Write a program, using trees, to assign the roll nos. to the students of your class as per their previous years result. i.e topper will be roll no. 1

## Theory

This program uses a Binary Search Tree (BST) to assign roll numbers to students based on their previous year's marks. The topper (highest marks) gets roll number 1, followed by the next highest, and so on.

A Binary Search Tree is a binary tree data structure where each node has at most two children, and for each node, all elements in its left subtree are less than the node, and all elements in its right subtree are greater than the node. This property allows for efficient insertion and traversal.

The program:
- Inserts students into a BST based on their marks (higher marks go to the right).
- Performs a reverse inorder traversal (right-root-left) to assign roll numbers starting from 1 for the highest marks.

This approach ensures that students are ranked in descending order of marks, with the topper getting roll number 1.

## Code

```cpp
/*
Write a program, using trees, to assign the roll nos. to the students of your
lass as per their previous years result. i.e topper will be roll no. 1
*/
#include <iostream>
#include <string>
using namespace std;

// Student structure
struct Student
{
    string name;
    int marks;

    Student(string n, int m) : name(n), marks(m) {}
};

// Node structure for BST (sorted by marks in descending order)
struct Node
{
    Student data;
    Node *left;
    Node *right;

    Node(Student s) : data(s), left(nullptr), right(nullptr) {}
};

// BST class for assigning roll numbers
class RollAssignBST
{
private:
    Node *root;

    // Insert helper: Higher marks go to left (descending order)
    Node *insertHelper(Node *node, Student s)
    {
        if (node == nullptr)
        {
            return new Node(s);
        }
        if (s.marks > node->data.marks)
        {
            node->left = insertHelper(node->left, s);
        }
        else
        {
            node->right = insertHelper(node->right, s);
        }
        return node;
    }

    // Inorder traversal to assign roll numbers (visits in descending order of marks)
    void assignRollsHelper(Node *node, int &rollNo)
    {
        if (node == nullptr)
        {
            return;
        }
        assignRollsHelper(node->left, rollNo); // Higher marks first
        cout << "Roll No. " << rollNo << ": " << node->data.name << " (Marks: " << node->data.marks << ")" << endl;
        rollNo++;
        assignRollsHelper(node->right, rollNo);
    }

public:
    RollAssignBST() : root(nullptr) {}

    ~RollAssignBST()
    {
        // Simplified: In real code, implement a function to delete all nodes
    }

    // Insert a student
    void insert(Student s)
    {
        root = insertHelper(root, s);
    }

    // Assign roll numbers starting from 1 (topper first)
    void assignRolls()
    {
        int rollNo = 1;
        assignRollsHelper(root, rollNo);
    }
};

int main()
{
    RollAssignBST bst;
    int n;

    cout << "Enter the number of students: ";
    cin >> n;

    for (int i = 0; i < n; ++i)
    {
        string name;
        int marks;
        cout << "Enter name and marks for student " << (i + 1) << ": ";
        cin >> name >> marks;
        bst.insert(Student(name, marks));
    }

    cout << "\nAssigned Roll Numbers (Topper first):\n";
    bst.assignRolls();

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS36.exe'
Enter the number of students: 4
Enter name and marks for student 1: kanchan 98
Enter name and marks for student 2: kapil 97
Enter name and marks for student 3: kajal 100
Enter name and marks for student 4: karan 95
Assigned Roll Numbers (Topper first):
Roll No. 1: kajal (Marks: 100)
Roll No. 2: kanchan (Marks: 98)
Roll No. 3: kapil (Marks: 97)
Roll No. 4: karan (Marks: 95)

```

## Conclusion

This program effectively uses a Binary Search Tree to rank students based on their marks and assign roll numbers accordingly. The BST insertion ensures that students are stored in a way that allows for efficient reverse inorder traversal to assign roll numbers from highest to lowest marks. This approach demonstrates the practical application of tree data structures in real-world scenarios like student ranking systems.
