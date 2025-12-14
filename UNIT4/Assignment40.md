# Assignment 40

## Title: Write a program to implement deletion operations in the product inventory system using a search tree. Each product must store the following information: ● Unique Product Code ● Product Name ● Price ● Quantity in Stock ● Date Received ● Expiration Date Implement the following operations: 1. Delete a product using its unique product code. 2. Delete all expired products based on the current date.

## Theory

This program extends the product inventory management system by implementing deletion operations in a Binary Search Tree (BST). The BST is organized by product code, allowing for efficient insertion, search, and deletion operations. The system supports two key deletion functionalities: deleting a specific product by its unique code and bulk deletion of all expired products based on the current date.

Key features of the system:
- **Product Information**: Each product stores unique code, name, price, quantity in stock, date received, and expiration date.
- **Insert Operation**: Adds new products to the BST, maintaining alphabetical order by product code.
- **Display All Products**: Uses inorder traversal to display all products sorted by code.
- **Delete by Code**: Removes a specific product from the BST using its unique product code, handling all BST deletion cases (leaf node, single child, two children).
- **Delete Expired Products**: Traverses the tree and removes all products whose expiration date has passed, based on current system time.

The BST provides O(log n) average time complexity for insertions and deletions, and traversals are O(n) for displaying all products. Date comparisons are handled using C++'s time functions to determine expired items.

## Code

```cpp
/*
Write a program to implement deletion operations in the product inventory system using a search tree.
 Each product must store the following information:
●	Unique Product Code
●	Product Name
●	Price
●	Quantity in Stock
●	Date Received
●	Expiration Date
Implement the following operations:
1.	Delete a product using its unique product code.

2.	Delete all expired products based on the current date.
*/
#include <iostream>
#include <string>
#include <vector>
#include <algorithm> // for std::swap

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
    Node *find_by_code(Node *node, const std::string &code)
    {
        if (!node)
            return nullptr;
        if (node->product->code == code)
            return node;
        Node *left = find_by_code(node->left, code);
        if (left)
            return left;
        return find_by_code(node->right, code);
    }
    Node *min_value_node(Node *node)
    {
        Node *current = node;
        while (current && current->left)
            current = current->left;
        return current;
    }
    Node *_delete(Node *node, const std::string &name)
    {
        if (!node)
            return node;
        if (name < node->product->name)
        {
            node->left = _delete(node->left, name);
        }
        else if (name > node->product->name)
        {
            node->right = _delete(node->right, name);
        }
        else
        {
            if (!node->left)
            {
                Node *temp = node->right;
                delete node->product;
                delete node;
                return temp;
            }
            else if (!node->right)
            {
                Node *temp = node->left;
                delete node->product;
                delete node;
                return temp;
            }
            Node *temp = min_value_node(node->right);
            std::swap(node->product, temp->product);
            node->right = _delete(node->right, temp->product->name);
        }
        return node;
    }
    void collect_expired(Node *node, const Date &today, std::vector<std::string> &codes)
    {
        if (node)
        {
            if (node->product->exp_date < today)
            {
                codes.push_back(node->product->code);
            }
            collect_expired(node->left, today, codes);
            collect_expired(node->right, today, codes);
        }
    }
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
    void delete_by_code(const std::string &code)
    {
        Node *node = find_by_code(root, code);
        if (node)
        {
            root = _delete(root, node->product->name);
        }
        else
        {
            std::cout << "Product with code " << code << " not found." << std::endl;
        }
    }
    void delete_all_expired(const Date &today)
    {
        std::vector<std::string> expired_codes;
        collect_expired(root, today, expired_codes);
        for (const auto &code : expired_codes)
        {
            delete_by_code(code);
        }
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

    std::cout << "Initial inventory:" << std::endl;
    inventory.inorder_display();

    // Delete a product by code
    std::cout << "\nDeleting product with code '002' (Banana):" << std::endl;
    inventory.delete_by_code("002");
    inventory.inorder_display();

    // Delete all expired products (assuming today is 2023-10-01)
    Date today(2023, 10, 1);
    std::cout << "\nDeleting all expired products:" << std::endl;
    inventory.delete_all_expired(today);
    inventory.inorder_display();

    // The destructor will handle deletion of remaining nodes and products

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS40.exe'

Initial inventory:
All items in inventory (inorder traversal):
Code: 001, Name: Apple, Price: 1.5, Qty: 100, Received: 2023-09-01, Expires: 2023-09-15
Code: 002, Name: Banana, Price: 0.75, Qty: 200, Received: 2023-09-05, Expires: 2023-10-10
Code: 003, Name: Cherry, Price: 2, Qty: 50, Received: 2023-08-20, Expires: 2023-09-10
Code: 004, Name: Date, Price: 3, Qty: 30, Received: 2023-09-10, Expires: 2023-11-01

Deleting product with code '002' (Banana):
All items in inventory (inorder traversal):
Code: 001, Name: Apple, Price: 1.5, Qty: 100, Received: 2023-09-01, Expires: 2023-09-15
Code: 003, Name: Cherry, Price: 2, Qty: 50, Received: 2023-08-20, Expires: 2023-09-10
Code: 004, Name: Date, Price: 3, Qty: 30, Received: 2023-09-10, Expires: 2023-11-01

Deleting all expired products:
All items in inventory (inorder traversal):
Code: 004, Name: Date, Price: 3, Qty: 30, Received: 2023-09-10, Expires: 2023-11-01

```

## Conclusion

This program demonstrates advanced BST operations by implementing comprehensive deletion functionalities in a product inventory system. By organizing products by unique codes, the BST enables efficient targeted deletions and bulk removal of expired items. The implementation handles all standard BST deletion cases, ensuring the tree remains balanced and functional after operations. This approach showcases how tree data structures can be used for dynamic inventory management, supporting both individual product removal and automated cleanup of expired stock, which is crucial for real-world inventory systems to maintain data integrity and prevent stock of outdated products.
