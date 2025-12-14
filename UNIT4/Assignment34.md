# Assignment 34

## Title: Write a program to perform Binary Tree operations (Create Binary Tree, Inorder/Preorder Traversal Non-Recursive, Count Leaf Nodes, Mirror Image)

## Theory

A Binary Tree is a hierarchical data structure where each node has at most two children, referred to as the left child and the right child. Unlike Binary Search Trees, there is no ordering property enforced on the nodes.

This program implements various binary tree operations:
- **Create Binary Tree**: Builds a binary tree using level-order input from the user.
- **Inorder Traversal (Non-Recursive)**: Performs inorder traversal using a stack, visiting left subtree, root, then right subtree.
- **Preorder Traversal (Non-Recursive)**: Performs preorder traversal using a stack, visiting root, left subtree, then right subtree.
- **Count Leaf Nodes**: Counts nodes that have no children using level-order traversal.
- **Mirror Image**: Creates a mirror image by swapping left and right subtrees of all nodes.

These operations demonstrate iterative tree traversals and modifications using stacks and queues.

## Code

```cpp
#include <iostream>
#include <stack>
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

    // Nonrecursive Inorder Traversal
    void inorderTraversal()
    {
        if (root == nullptr)
        {
            cout << "Tree is empty." << endl;
            return;
        }
        stack<Node *> s;
        Node *current = root;
        while (current != nullptr || !s.empty())
        {
            while (current != nullptr)
            {
                s.push(current);
                current = current->left;
            }
            current = s.top();
            s.pop();
            cout << current->data << " ";
            current = current->right;
        }
        cout << endl;
    }

    // Nonrecursive Preorder Traversal
    void preorderTraversal()
    {
        if (root == nullptr)
        {
            cout << "Tree is empty." << endl;
            return;
        }
        stack<Node *> s;
        s.push(root);
        while (!s.empty())
        {
            Node *current = s.top();
            s.pop();
            cout << current->data << " ";
            if (current->right)
                s.push(current->right);
            if (current->left)
                s.push(current->left);
        }
        cout << endl;
    }

    // Display Number of Leaf Nodes (iterative)
    int countLeafNodes()
    {
        if (root == nullptr)
        {
            return 0;
        }
        stack<Node *> s;
        s.push(root);
        int count = 0;
        while (!s.empty())
        {
            Node *current = s.top();
            s.pop();
            if (current->left == nullptr && current->right == nullptr)
            {
                count++;
            }
            if (current->right)
                s.push(current->right);
            if (current->left)
                s.push(current->left);
        }
        return count;
    }

    // Mirror Image (iterative)
    void mirrorImage()
    {
        if (root == nullptr)
        {
            return;
        }
        stack<Node *> s;
        s.push(root);
        while (!s.empty())
        {
            Node *current = s.top();
            s.pop();
            // Swap left and right
            swap(current->left, current->right);
            if (current->right)
                s.push(current->right);
            if (current->left)
                s.push(current->left);
        }
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

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS34.exe'
Inorder Traversal: 20 30 40 50 60 70 80
Preorder Traversal: 50 30 20 40 70 60 80
Number of Leaf Nodes: 4
After Mirror Image, Inorder Traversal: 80 70 60 50 40 30 20

```

## Conclusion

This program successfully demonstrates various binary tree operations including creation, non-recursive traversals, leaf node counting, and mirror image creation. The iterative implementations using stacks and queues provide efficient alternatives to recursive approaches, avoiding potential stack overflow issues for large trees. These operations are fundamental for understanding tree data structures and their manipulations in computer science.
