# Assignment 32

## Write a program to perform Binary Search Tree (BST) operations (Count the total number of nodes, Compute the height of the BST, Mirror Image)

## Theory

A Binary Search Tree (BST) is a hierarchical data structure where each node has at most two children, referred to as the left and right child. The key property is that for any node, all elements in its left subtree are less than the node, and all elements in its right subtree are greater than the node.

This program implements additional BST operations beyond basic insertion and traversal:
- **Count Nodes**: Recursively counts the total number of nodes in the tree.
- **Compute Height**: Calculates the height of the tree, defined as the longest path from the root to a leaf.
- **Mirror Image**: Creates a mirror image of the tree by swapping left and right subtrees recursively.

These operations demonstrate tree traversal techniques and property calculations essential for understanding tree structures.

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

    // Helper function to count total nodes
    int countNodesHelper(Node *node)
    {
        if (node == nullptr)
        {
            return 0;
        }
        return 1 + countNodesHelper(node->left) + countNodesHelper(node->right);
    }

    // Helper function to compute height
    int heightHelper(Node *node)
    {
        if (node == nullptr)
        {
            return 0;
        }
        int leftHeight = heightHelper(node->left);
        int rightHeight = heightHelper(node->right);
        return 1 + max(leftHeight, rightHeight);
    }

    // Helper function to create mirror image
    void mirrorHelper(Node *node)
    {
        if (node == nullptr)
        {
            return;
        }
        // Swap left and right subtrees
        swap(node->left, node->right);
        // Recurse on both subtrees
        mirrorHelper(node->left);
        mirrorHelper(node->right);
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

    // Levelwise display (BFS)
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
            cout << endl; // New line after each level
        }
    }

    // Count the total number of nodes
    int countNodes()
    {
        return countNodesHelper(root);
    }

    // Compute the height of the BST
    int height()
    {
        return heightHelper(root);
    }

    // Create mirror image of the BST
    void mirror()
    {
        mirrorHelper(root);
    }
};

int main()
{
    BST tree;

    // Example operations
    tree.insert(50);
    tree.insert(30);
    tree.insert(70);
    tree.insert(20);
    tree.insert(40);
    tree.insert(60);
    tree.insert(80);

    cout << "Levelwise display after insertions:" << endl;
    tree.levelwiseDisplay();

    cout << "\nTotal number of nodes: " << tree.countNodes() << endl;
    cout << "Height of the BST: " << tree.height() << endl;

    tree.mirror();
    cout << "\nLevelwise display after mirroring:" << endl;
    tree.levelwiseDisplay();

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS32.exe'
Levelwise display after insertions:
50
30 70
20 40 60 80
Total number of nodes: 7
Height of the BST: 3
Levelwise display after mirroring:
50
70 30
80 60 40 20

```

## Conclusion

This program effectively demonstrates advanced Binary Search Tree operations including node counting, height calculation, and mirror image creation. The recursive implementations ensure correct traversal and modification of the tree structure. These operations are crucial for analyzing tree properties and performing transformations, showcasing the versatility of BSTs in data management and algorithmic problem-solving.
