# Assignment 31 
## Title:  Write a program to perform Binary Search Tree (BST) operations (Create, Insert, Delete, Levelwise display)

## Theory

A Binary Search Tree (BST) is a data structure in which each node has at most two children, referred to as the left child and the right child. For each node, all elements in its left subtree are less than the node, and all elements in its right subtree are greater than the node. This property allows for efficient searching, insertion, and deletion operations.

The program implements BST operations using a linked list representation:
- **Create**: Builds the BST by inserting nodes from user input.
- **Insert**: Adds a new node while maintaining the BST property.
- **Delete**: Removes a node and restructures the tree to preserve the BST property.
- **Levelwise Display**: Performs a breadth-first traversal (BFS) to display nodes level by level.

## Code

```cpp
#include <iostream>
#include <queue>
using namespace std;

struct Node
{
    int data;
    Node *left;
    Node *right;

    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

class BST
{
private:
    Node *root;

    Node *insertHelper(Node *node, int val)
    {
        if (node == nullptr)
        {
            return new Node(val);
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

    Node *findMin(Node *node)
    {
        while (node->left != nullptr)
        {
            node = node->left;
        }
        return node;
    }

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

            if (node->left == nullptr)
            {
                Node *temp = node->right;
                delete node;
                return temp;
            }
            else if (node->right == nullptr)
            {
                Node *temp = node->left;
                delete node;
                return temp;
            }

            Node *temp = findMin(node->right);
            node->data = temp->data;
            node->right = deleteHelper(node->right, temp->data);
        }
        return node;
    }

public:
    BST() : root(nullptr) {}

    ~BST()
    {
    }

    void insert(int val)
    {
        root = insertHelper(root, val);
    }

    void deleteNode(int val)
    {
        root = deleteHelper(root, val);
    }

    void levelwiseDisplay()
    {
        if (root == nullptr)
        {
            cout << "Tree is empty." << endl;
            return;
        }
        queue<Node *> q;
        q.push(root);
        while (!q.empty())
        {
            int levelSize = q.size();
            for (int i = 0; i < levelSize; ++i)
            {
                Node *current = q.front();
                q.pop();
                cout << current->data << " ";
                if (current->left)
                    q.push(current->left);
                if (current->right)
                    q.push(current->right);
            }
            cout << endl;
        }
    }
};

int main()
{
    BST tree;

    tree.insert(50);
    tree.insert(30);
    tree.insert(70);
    tree.insert(20);
    tree.insert(40);
    tree.insert(60);
    tree.insert(80);

    cout << "Levelwise display after insertions:" << endl;
    tree.levelwiseDisplay();

    tree.deleteNode(20);
    cout << "\nAfter deleting 20:" << endl;
    tree.levelwiseDisplay();

    tree.deleteNode(30);
    cout << "\nAfter deleting 30:" << endl;
    tree.levelwiseDisplay();

    tree.deleteNode(50);
    cout << "\nAfter deleting 50:" << endl;
    tree.levelwiseDisplay();

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS31.exe'
Levelwise display after insertions:
50
30 70
20 40 60 80
After deleting 20:
50
30 70
40 60 80
After deleting 30:
50
40 70
60 80
After deleting 50:
60
40 70
80

```

## Conclusion

This program successfully demonstrates the implementation of a Binary Search Tree with operations for creation, insertion, deletion, and levelwise display. The BST maintains its properties through recursive operations, ensuring efficient data management. The levelwise display uses BFS for clear visualization, and inorder traversal confirms the sorted order. This assignment highlights the importance of tree data structures in computer science for organized data storage and retrieval.
