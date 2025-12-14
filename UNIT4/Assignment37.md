# Assignment 37

## Write a program to illustrate operations on a BST holding numeric keys. The menu must include: • Insert • Delete • Find • Show

## Theory

A Binary Search Tree (BST) is a binary tree data structure where each node has at most two children, and for each node, all elements in its left subtree are less than the node, and all elements in its right subtree are greater than the node. This property allows for efficient insertion, deletion, and search operations.

This program implements basic BST operations:
- **Insert**: Adds a new key to the BST while maintaining the BST property.
- **Delete**: Removes a key from the BST, handling cases for leaf nodes, nodes with one child, and nodes with two children.
- **Find**: Searches for a key in the BST.
- **Show**: Displays the BST using inorder traversal, which visits nodes in sorted order.

These operations demonstrate the fundamental functionality of BSTs, which are essential for efficient data storage and retrieval in computer science.

## Code

```cpp
#include <iostream>
#include <queue>
using namespace std;

// Node structure for BST
struct Node
{
    int data;
    Node *left;
    Node *right;

    // Constructor for dynamic allocation
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

// BST class
class BST
{
private:
    Node *root;

    // Helper function to insert a node
    Node *insertHelper(Node *node, int val)
    {
        if (node == nullptr)
        {
            return new Node(val); // Dynamic allocation
        }
        if (val < node->data)
        {
            node->left = insertHelper(node->left, val);
        }
        else if (val > node->data)
        {
            node->right = insertHelper(node->right, val);
        }
        return node;
    }

    // Helper function to find the minimum value node
    Node *findMin(Node *node)
    {
        while (node->left != nullptr)
        {
            node = node->left;
        }
        return node;
    }

    // Helper function to delete a node
    Node *deleteHelper(Node *node, int val)
    {
        if (node == nullptr)
        {
            return nullptr;
        }
        if (val < node->data)
        {
            node->left = deleteHelper(node->left, val);
        }
        else if (val > node->data)
        {
            node->right = deleteHelper(node->right, val);
        }
        else
        {
            // Node to be deleted found
            if (node->left == nullptr)
            {
                Node *temp = node->right;
                delete node; // Free memory
                return temp;
            }
            else if (node->right == nullptr)
            {
                Node *temp = node->left;
                delete node; // Free memory
                return temp;
            }
            // Node with two children
            Node *temp = findMin(node->right);
            node->data = temp->data;
            node->right = deleteHelper(node->right, temp->data);
        }
        return node;
    }

    // Helper function to find a value
    bool findHelper(Node *node, int val)
    {
        if (node == nullptr)
        {
            return false;
        }
        if (val == node->data)
        {
            return true;
        }
        else if (val < node->data)
        {
            return findHelper(node->left, val);
        }
        else
        {
            return findHelper(node->right, val);
        }
    }

    // Helper function for inorder display
    void inorderHelper(Node *node)
    {
        if (node == nullptr)
        {
            return;
        }
        inorderHelper(node->left);
        cout << node->data << " ";
        inorderHelper(node->right);
    }

public:
    // Constructor to create an empty BST
    BST() : root(nullptr) {}

    // Destructor to free memory (simplified)
    ~BST()
    {
        // In a real implementation, implement a function to delete all nodes
    }

    // Insert a value into the BST
    void insert(int val)
    {
        root = insertHelper(root, val);
    }

    // Delete a value from the BST
    void deleteNode(int val)
    {
        root = deleteHelper(root, val);
    }

    // Find a value in the BST
    bool find(int val)
    {
        return findHelper(root, val);
    }

    // Show the BST (inorder traversal)
    void show()
    {
        if (root == nullptr)
        {
            cout << "Tree is empty." << endl;
            return;
        }
        cout << "BST (Inorder): ";
        inorderHelper(root);
        cout << endl;
    }
};

int main()
{
    BST tree;
    int choice, val;

    do
    {
        cout << "\nBST Operations Menu:" << endl;
        cout << "1. Insert" << endl;
        cout << "2. Delete" << endl;
        cout << "3. Find" << endl;
        cout << "4. Show" << endl;
        cout << "5. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            cout << "Enter value to insert: ";
            cin >> val;
            tree.insert(val);
            cout << "Inserted " << val << "." << endl;
            break;
        case 2:
            cout << "Enter value to delete: ";
            cin >> val;
            tree.deleteNode(val);
            cout << "Deleted " << val << " (if it existed)." << endl;
            break;
        case 3:
            cout << "Enter value to find: ";
            cin >> val;
            if (tree.find(val))
            {
                cout << val << " found in the BST." << endl;
            }
            else
            {
                cout << val << " not found in the BST." << endl;
            }
            break;
        case 4:
            tree.show();
            break;
        case 5:
            cout << "Exiting..." << endl;
            break;
        default:
            cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 5);

    return 0;
}


```

## Output
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS37.exe'
BST Operations Menu:
1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter your choice: 1
Enter value to insert: 10
Inserted 10.

BST Operations Menu:
1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter your choice: 1
Enter value to insert: 20
Inserted 20.

BST Operations Menu:
1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter your choice: 4
BST (Inorder): 10 20

BST Operations Menu:
1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter your choice: 3
Enter value to find: 10
10 found in the BST.

BST Operations Menu:
1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter your choice: 2
Enter value to delete: 10
Deleted 10 (if it existed).

BST Operations Menu:
1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter your choice: 4
BST (Inorder): 20

BST Operations Menu:
1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter your choice: 5
Exiting...

```

## Conclusion

This program successfully demonstrates the core operations of a Binary Search Tree, including insertion, deletion, search, and display. The BST maintains its properties through these operations, ensuring that elements are stored in a way that allows for efficient searching and sorting. These fundamental operations are crucial for understanding tree-based data structures and their applications in computer science, such as in databases, file systems, and search algorithms.
