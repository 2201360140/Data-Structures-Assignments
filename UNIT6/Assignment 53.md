# Assignment 53  

## Title  
**Implement collision resolution using linked lists.**  


---

## Theory  
Collision resolution using linked lists is a manual implementation of separate chaining, where each bucket in the hash table is a linked list. Instead of using standard library containers like std::list, this approach defines a custom Node class to build the linked lists dynamically.

When a collision occurs (multiple keys hash to the same index), the new key is inserted into the linked list at that index. This allows unlimited keys per bucket without resizing the array, as long as memory permits.

**How it Works:**  
- Each index in the hash table array points to the head of a linked list (or nullptr if empty).  
- Insertion: Create a new node and insert it at the beginning of the list for simplicity (O(1) time).  
- Search: Traverse the list at the hashed index until the key is found or the end is reached.  
- Deletion: Traverse the list, find the node, and unlink it, then deallocate memory.  

**Advantages:**  
- Handles collisions gracefully without clustering.  
- No fixed limit on the number of keys per bucket.  
- Memory is allocated dynamically as needed.  
- Easy to implement with basic pointer operations.  

**Disadvantages:**  
- Additional memory overhead for node pointers and dynamic allocation/deallocation.  
- Potential for memory leaks if not managed properly.  
- Worst-case performance degrades if many keys collide (poor hash function).  
- Cache inefficiency due to scattered memory access.  

This method is educational for understanding low-level data structures and is similar to how hash tables were implemented before standard libraries provided built-in support. It emphasizes manual memory management in C++.

---

## Code  
```cpp
#include <iostream>
#include <vector>
#include <string>

// Node structure for the linked list
struct Node
{
    std::string key;
    int value;
    Node *next;

    // Constructor
    Node(std::string k, int v) : key(k), value(v), next(nullptr) {}
};

// HashTable class
class HashTable
{
private:
    std::vector<Node *> table; // Vector of pointers to linked lists
    int size;                  // Number of buckets

    // Hash function: Sum of ASCII values modulo size
    int hashFunction(const std::string &key)
    {
        int sum = 0;
        for (char c : key)
        {
            sum += static_cast<int>(c);
        }
        return sum % size;
    }

public:
    // Constructor: Initialize table with given size
    HashTable(int s) : size(s)
    {
        table.resize(size, nullptr);
    }

    // Destructor: Clean up all dynamically allocated nodes
    ~HashTable()
    {
        for (int i = 0; i < size; ++i)
        {
            Node *current = table[i];
            while (current)
            {
                Node *temp = current;
                current = current->next;
                delete temp;
            }
        }
    }

    // Insert a key-value pair
    void insert(const std::string &key, int value)
    {
        int index = hashFunction(key);
        Node *newNode = new Node(key, value); // Dynamic allocation

        // If bucket is empty, set as head
        if (!table[index])
        {
            table[index] = newNode;
            return;
        }

        // Traverse to the end of the list and append
        Node *current = table[index];
        while (current->next)
        {
            // If key already exists, update value and delete newNode
            if (current->key == key)
            {
                current->value = value;
                delete newNode;
                return;
            }
            current = current->next;
        }
        // Check the last node
        if (current->key == key)
        {
            current->value = value;
            delete newNode;
            return;
        }
        // Append new node
        current->next = newNode;
    }

    // Search for a key and return its value (or -1 if not found)
    int search(const std::string &key)
    {
        int index = hashFunction(key);
        Node *current = table[index];
        while (current)
        {
            if (current->key == key)
            {
                return current->value;
            }
            current = current->next;
        }
        return -1; // Not found
    }

    // Delete a key-value pair
    void remove(const std::string &key)
    {
        int index = hashFunction(key);
        Node *current = table[index];
        Node *prev = nullptr;

        while (current)
        {
            if (current->key == key)
            {
                if (prev)
                {
                    prev->next = current->next;
                }
                else
                {
                    table[index] = current->next;
                }
                delete current; // Dynamic deallocation
                return;
            }
            prev = current;
            current = current->next;
        }
        // Key not found, do nothing
    }

    // Print the hash table (for debugging)
    void print()
    {
        for (int i = 0; i < size; ++i)
        {
            std::cout << "Bucket " << i << ": ";
            Node *current = table[i];
            while (current)
            {
                std::cout << "(" << current->key << ", " << current->value << ") -> ";
                current = current->next;
            }
            std::cout << "nullptr" << std::endl;
        }
    }
};

// Example usage
int main()
{
    HashTable ht(10); // Hash table with 10 buckets

    // Insert some key-value pairs
    ht.insert("apple", 10);
    ht.insert("banana", 20);
    ht.insert("cherry", 30);
    ht.insert("date", 40); // This might collide with others depending on hash

    // Print the table
    std::cout << "Hash Table after insertions:" << std::endl;
    ht.print();

    // Search
    std::cout << "\nSearch 'banana': " << ht.search("banana") << std::endl;
    std::cout << "Search 'grape': " << ht.search("grape") << std::endl;

    // Remove
    ht.remove("banana");
    std::cout << "\nAfter removing 'banana':" << std::endl;
    ht.print();

    return 0;
}


```
---
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS53.exe'
Hash Table after insertions:
Bucket 0: (apple, 10) -> nullptr
Bucket 1: nullptr
Bucket 2: nullptr
Bucket 3: (cherry, 30) -> nullptr
Bucket 4: (date, 40) -> nullptr
Bucket 5: nullptr
Bucket 6: nullptr
Bucket 7: nullptr
Bucket 8: nullptr
Bucket 9: (banana, 20) -> nullptr
Search 'banana': 20
Search 'grape': -1
After removing 'banana':
Bucket 0: (apple, 10) -> nullptr
Bucket 1: nullptr
Bucket 2: nullptr
Bucket 3: (cherry, 30) -> nullptr
Bucket 4: (date, 40) -> nullptr
Bucket 5: nullptr
Bucket 6: nullptr
Bucket 7: nullptr
Bucket 8: nullptr
Bucket 9: nullptr


---
## Conclusion  
In this program, we implemented a hash table with collision resolution using manually managed linked lists in C++. Each bucket is a linked list built from a custom Node class, allowing multiple keys to coexist at the same index. This approach demonstrates low-level memory management and provides efficient collision handling, with average-case O(1) operations assuming uniform hashing. It is particularly useful for educational purposes to understand pointer-based data structures, though it requires careful handling to avoid memory leaks.
