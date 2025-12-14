# Assignment 39

## Title: Write a program to implement a product inventory management system for a shop using a search tree data structure. Each product must store the following information: 
-  Unique Product Code
-  Product Name 
-  Price 
-  Quantity in Stock 
-  Date Received 
-  Expiration Date 
- Implement the following operations: 1. Insert a product into the tree ( organized by product name). 2. Display all items in the inventory using inorder traversal. 3. List expired items in prefix (preorder) order of their names.

## Theory

This program implements a product inventory management system using a Binary Search Tree (BST) data structure. Each product is stored as a node in the BST, with the product name used as the key for sorting. The BST ensures that products are organized alphabetically by name, allowing for efficient insertion and traversal operations.

Key features of the system:
- **Product Information**: Each product stores unique code, name, price, quantity in stock, date received, and expiration date.
- **Insert Operation**: Adds new products to the BST, maintaining alphabetical order by product name.
- **Display All Products**: Uses inorder traversal to display all products sorted by name.
- **List Expired Products**: Uses preorder traversal to list products that have expired, based on the current date comparison with expiration dates.

The BST provides O(log n) average time complexity for insertions, and traversals are O(n) for displaying all products. Date comparisons are handled using C++'s time functions to determine expired items.

## Code

```cpp
#include <iostream>
#include <string>
#include <vector>

struct Date
{
    int year, month, day;
    Date(int y, int m, int d) : year(y), month(m), day(d) {}
    bool operator<(const Date &other) const
    {
        if (year != other.year)
            return year < other.year;
        if (month != other.month)
            return month < other.month;
        return day < other.day;
    }
    friend std::ostream &operator<<(std::ostream &os, const Date &d)
    {
        os << d.year << "-" << (d.month < 10 ? "0" : "") << d.month << "-" << (d.day < 10 ? "0" : "") << d.day;
        return os;
    }
};

class Product
{
public:
    std::string code;
    std::string name;
    double price;
    int qty;
    Date date_rec;
    Date exp_date;
    Product(std::string c, std::string n, double p, int q, Date dr, Date ed)
        : code(c), name(n), price(p), qty(q), date_rec(dr), exp_date(ed) {}
};

class Node
{
public:
    Product *product;
    Node *left;
    Node *right;
    Node(Product *p) : product(p), left(nullptr), right(nullptr) {}
};

class BST
{
private:
    Node *root;
    void _insert(Node *&node, Product *product)
    {
        if (node == nullptr)
        {
            node = new Node(product);
        }
        else if (product->name < node->product->name)
        {
            _insert(node->left, product);
        }
        else
        {
            _insert(node->right, product);
        }
    }
    void _inorder(Node *node)
    {
        if (node)
        {
            _inorder(node->left);
            std::cout << "Code: " << node->product->code << ", Name: " << node->product->name
                      << ", Price: " << node->product->price << ", Qty: " << node->product->qty
                      << ", Received: " << node->product->date_rec << ", Expires: " << node->product->exp_date << std::endl;
            _inorder(node->right);
        }
    }
    void _list_expired_preorder(Node *node, const Date &today)
    {
        if (node)
        {
            if (node->product->exp_date < today)
            {
                std::cout << "Expired: " << node->product->name << std::endl;
            }
            _list_expired_preorder(node->left, today);
            _list_expired_preorder(node->right, today);
        }
    }
    void _delete_tree(Node *node)
    {
        if (node)
        {
            _delete_tree(node->left);
            _delete_tree(node->right);
            delete node->product;
            delete node;
        }
    }

public:
    BST() : root(nullptr) {}
    ~BST()
    {
        _delete_tree(root);
    }
    void insert(Product *product)
    {
        _insert(root, product);
    }
    void inorder_display()
    {
        std::cout << "All items in inventory (inorder traversal):" << std::endl;
        _inorder(root);
    }
    void list_expired_preorder(const Date &today)
    {
        std::cout << "Expired items in preorder traversal:" << std::endl;
        _list_expired_preorder(root, today);
    }
};

int main()
{
    BST inventory;

    // Sample products (assuming current date is around 2023-10-01 for demonstration)
    // Some expired, some not
    Product *p1 = new Product("001", "Apple", 1.50, 100, Date(2023, 9, 1), Date(2023, 9, 15));   // Expired
    Product *p2 = new Product("002", "Banana", 0.75, 200, Date(2023, 9, 5), Date(2023, 10, 10)); // Not expired
    Product *p3 = new Product("003", "Cherry", 2.00, 50, Date(2023, 8, 20), Date(2023, 9, 10));  // Expired
    Product *p4 = new Product("004", "Date", 3.00, 30, Date(2023, 9, 10), Date(2023, 11, 1));    // Not expired

    inventory.insert(p1);
    inventory.insert(p2);
    inventory.insert(p3);
    inventory.insert(p4);

    // Display all items in inorder
    inventory.inorder_display();

    // List expired items in preorder (assuming today is 2023-10-01)
    Date today(2023, 10, 1);
    inventory.list_expired_preorder(today);

    // The destructor will handle deletion of nodes and products

    return 0;
}


```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS39.exe'
All items in inventory (inorder traversal):
Code: 001, Name: Apple, Price: 1.5, Qty: 100, Received: 2023-09-01, Expires:
2023-09-15
Code: 002, Name: Banana, Price: 0.75, Qty: 200, Received: 2023-09-05, Expires:
2023-10-10
Code: 003, Name: Cherry, Price: 2, Qty: 50, Received: 2023-08-20, Expires: 2023-
09-10
Code: 004, Name: Date, Price: 3, Qty: 30, Received: 2023-09-10, Expires: 2023-11-
01
Expired items in preorder traversal:
Expired: Apple
Expired: Cherry

```

## Conclusion

This program demonstrates the practical application of Binary Search Trees in managing a product inventory system. By organizing products alphabetically by name, the BST enables efficient insertion and sorted display of inventory items. The preorder traversal for expired products provides a logical way to list items that need attention, prioritizing root-level products first. This implementation showcases how tree data structures can be used for real-world inventory management, ensuring products are easily searchable and sortable while tracking expiration dates for quality control.
