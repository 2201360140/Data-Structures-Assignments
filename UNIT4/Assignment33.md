# Assignment 33

## Write a Program to create a Binary Tree Search and Find Minimum/Maximum in BST

## Theory

A Binary Search Tree (BST) is a node-based binary tree data structure which has the following properties:
- The left subtree of a node contains only nodes with keys lesser than the node's key.
- The right subtree of a node contains only nodes with keys greater than the node's key.
- The left and right subtree each must also be a binary search tree.

This program creates a BST and provides functionality to find the minimum and maximum values in the tree. The minimum value is found by traversing to the leftmost node, while the maximum value is found by traversing to the rightmost node. These operations are O(h) where h is the height of the tree.

## Code

```cpp
#include <iostream>
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
    Node *findMinHelper(Node *node)
    {
        if (node == nullptr)
        {
            return nullptr;
        }
        while (node->left != nullptr)
        {
            node = node->left;
        }
        return node;
    }

    // Helper function to find the maximum value node
    Node *findMaxHelper(Node *node)
    {
        if (node == nullptr)
        {
            return nullptr;
        }
        while (node->right != nullptr)
        {
            node = node->right;
        }
        return node;
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

    // Find the minimum value in the BST
    int findMin()
    {
        Node *minNode = findMinHelper(root);
        if (minNode == nullptr)
        {
            throw runtime_error("Tree is empty");
        }
        return minNode->data;
    }

    // Find the maximum value in the BST
    int findMax()
    {
        Node *maxNode = findMaxHelper(root);
        if (maxNode == nullptr)
        {
            throw runtime_error("Tree is empty");
        }
        return maxNode->data;
    }
};

int main()
{
    BST tree;

    // Example insertions to create the BST
    tree.insert(50);
    tree.insert(30);
    tree.insert(70);
    tree.insert(20);
    tree.insert(40);
    tree.insert(60);
    tree.insert(80);

    try
    {
        cout << "Minimum value in BST: " << tree.findMin() << endl;
        cout << "Maximum value in BST: " << tree.findMax() << endl;
    }
    catch (const runtime_error &e)
    {
        cout << e.what() << endl;
    }

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS33.exe'
Minimum value in BST: 20
Maximum value in BST: 80

```

## Conclusion

This program successfully implements a Binary Search Tree with insertion and minimum/maximum finding capabilities. The BST property ensures that finding min/max is efficient by traversing to the leftmost or rightmost nodes respectively. The inorder traversal confirms the sorted nature of the BST. This assignment demonstrates fundamental BST operations and their practical implementation in C++.
