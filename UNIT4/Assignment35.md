# Assignment 35

## Title: Write a Program to create a Binary Tree and perform the following Recursive operations on it. a. Inorder Traversal b. Preorder Traversal c. Display Number of Leaf Nodes d. Mirror Image

## Theory

A Binary Tree is a hierarchical data structure where each node has at most two children, referred to as the left child and the right child. Unlike Binary Search Trees, there is no ordering property enforced on the nodes.

This program implements recursive operations on a binary tree:
- **Create Binary Tree**: Builds a binary tree using recursive input from the user.
- **Inorder Traversal (Recursive)**: Visits left subtree, root, then right subtree (Left → Root → Right).
- **Preorder Traversal (Recursive)**: Visits root, left subtree, then right subtree (Root → Left → Right).
- **Count Leaf Nodes**: Recursively counts nodes that have no children.
- **Mirror Image**: Recursively swaps left and right subtrees to create a mirror image.

These recursive operations demonstrate fundamental tree traversals and modifications in computer science.

## Code

```cpp
#include <iostream>
using namespace std;

// Node structure for Binary Tree
struct Node
{
    int data;
    Node *left;
    Node *right;

    // Constructor for dynamic allocation
    Node(int val) : data(val), left(nullptr), right(nullptr) {}
};

// Binary Tree class
class BT
{
private:
    Node *root;

    // Helper function to insert a node (for building the tree, assuming BST property for simplicity)
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

    // Recursive Inorder Traversal
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

    // Recursive Preorder Traversal
    void preorderHelper(Node *node)
    {
        if (node == nullptr)
        {
            return;
        }
        cout << node->data << " ";
        preorderHelper(node->left);
        preorderHelper(node->right);
    }

    // Recursive function to count leaf nodes
    int countLeafHelper(Node *node)
    {
        if (node == nullptr)
        {
            return 0;
        }
        if (node->left == nullptr && node->right == nullptr)
        {
            return 1;
        }
        return countLeafHelper(node->left) + countLeafHelper(node->right);
    }

    // Recursive function to create mirror image
    void mirrorHelper(Node *node)
    {
        if (node == nullptr)
        {
            return;
        }
        // Swap left and right
        swap(node->left, node->right);
        // Recurse on both subtrees
        mirrorHelper(node->left);
        mirrorHelper(node->right);
    }

public:
    // Constructor to create an empty Binary Tree
    BT() : root(nullptr) {}

    // Destructor to free memory (simplified)
    ~BT()
    {
        // In a real implementation, implement a function to delete all nodes
    }

    // Insert a value into the Binary Tree (maintaining BST property for example)
    void insert(int val)
    {
        root = insertHelper(root, val);
    }

    // Recursive Inorder Traversal
    void inorderTraversal()
    {
        if (root == nullptr)
        {
            cout << "Tree is empty." << endl;
            return;
        }
        inorderHelper(root);
        cout << endl;
    }

    // Recursive Preorder Traversal
    void preorderTraversal()
    {
        if (root == nullptr)
        {
            cout << "Tree is empty." << endl;
            return;
        }
        preorderHelper(root);
        cout << endl;
    }

    // Display Number of Leaf Nodes
    int countLeafNodes()
    {
        return countLeafHelper(root);
    }

    // Mirror Image
    void mirrorImage()
    {
        mirrorHelper(root);
    }
};

int main()
{
    BT tree;

    // Example insertions to create the Binary Tree
    tree.insert(50);
    tree.insert(30);
    tree.insert(70);
    tree.insert(20);
    tree.insert(40);
    tree.insert(60);
    tree.insert(80);

    cout << "Inorder Traversal: ";
    tree.inorderTraversal();

    cout << "Preorder Traversal: ";
    tree.preorderTraversal();

    cout << "Number of Leaf Nodes: " << tree.countLeafNodes() << endl;

    tree.mirrorImage();
    cout << "After Mirror Image, Inorder Traversal: ";
    tree.inorderTraversal();

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS35.exe'
Inorder Traversal: 20 30 40 50 60 70 80
Preorder Traversal: 50 30 20 40 70 60 80
Number of Leaf Nodes: 4
After Mirror Image, Inorder Traversal: 80 70 60 50 40 30 20

```

## Conclusion

This program successfully demonstrates recursive binary tree operations including creation, traversals, leaf node counting, and mirror image creation. The recursive implementations provide elegant solutions for tree manipulations, though they may encounter stack overflow for very deep trees. Understanding these recursive patterns is crucial for mastering tree data structures and algorithms in computer science.
