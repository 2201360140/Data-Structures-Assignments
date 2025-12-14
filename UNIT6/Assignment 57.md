# Assignment 57

## Title
WAP to simulate a faculty database as a hash table. Search a particular faculty by using MOD as a hash function for linear probing with chaining with replacement method of collision handling technique. Assume suitable data for faculty record. Give the code

## Theory
A hash table is a data structure that implements an associative array abstract data type, a structure that can map keys to values. It uses a hash function to compute an index into an array of buckets or slots, from which the desired value can be found.

In this program, we simulate a faculty database using a hash table with chaining for collision handling, specifically with replacement. Each bucket in the hash table is a linked list (std::list in C++), allowing multiple records to be stored at the same index.

The hash function used is the MOD operation: hash(key) = key % size, where size is the table size (10 in this case).

For collision handling, we use chaining with replacement. When inserting a record, if a record with the same ID already exists in the list at the hashed index, it is replaced with the new record. Otherwise, the new record is appended to the list. This is different from without replacement, where duplicates would be added without replacing.

Linear probing is not used here; instead, chaining handles collisions by maintaining a list at each index.

### Example of Hash Function and Chaining with Replacement
Suppose we have a hash table of size 10, and we insert faculty with ID 101.
- Hash function: 101 % 10 = 1, insert at index 1.
- If we insert another with ID 101, it replaces the existing one at index 1.

### Diagramatic Representation
```
Hash Table (Size 10):
Index: 0 -> [F1] -> [F2]
       1 -> [F3] (replaced if duplicate ID)
       2 -> nullptr
       ...
```
Where each index points to a linked list of faculty records, with replacement for duplicates.

The program allows inserting faculty records into the hash table (with replacement for duplicates) and searching for a faculty by ID by traversing the list at the hashed index.

## Code
```cpp
#include <iostream>
#include <vector>
#include <string>

// Faculty record structure
struct Faculty
{
    int id;
    std::string name;
    std::string department;
    int experience; // Years of experience

    // Constructor
    Faculty(int i = -1, std::string n = "", std::string d = "", int e = 0)
        : id(i), name(n), department(d), experience(e) {}
};

// Node structure for the linked list
struct Node
{
    Faculty faculty;
    Node *next;

    // Constructor
    Node(Faculty f) : faculty(f), next(nullptr) {}
};

// HashTable class using separate chaining with replacement
class FacultyHashTable
{
private:
    std::vector<Node *> table; // Vector of pointers to linked lists
    int size;                  // Number of buckets

    // Hash function: MOD method (ID modulo size)
    int hashFunction(int id)
    {
        return id % size;
    }

public:
    // Constructor: Initialize table with given size
    FacultyHashTable(int s) : size(s)
    {
        table.resize(size, nullptr);
    }

    // Destructor: Clean up all dynamically allocated nodes
    ~FacultyHashTable()
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

    // Insert a faculty record (replace if ID exists, else append)
    void insert(int id, const std::string &name, const std::string &department, int experience)
    {
        int index = hashFunction(id);
        Faculty newFaculty(id, name, department, experience);
        Node *newNode = new Node(newFaculty); // Dynamic allocation

        // If bucket is empty, set as head
        if (!table[index])
        {
            table[index] = newNode;
            return;
        }

        // Traverse the list to check for existing ID (replace if found)
        Node *current = table[index];
        while (current)
        {
            if (current->faculty.id == id)
            {
                // Replace (update) existing record
                current->faculty.name = name;
                current->faculty.department = department;
                current->faculty.experience = experience;
                delete newNode; // No need for new node
                return;
            }
            if (!current->next)
                break; // Reached end
            current = current->next;
        }
        // Append new node if ID not found
        current->next = newNode;
    }

    // Search for a faculty by ID
    Faculty *search(int id)
    {
        int index = hashFunction(id);
        Node *current = table[index];
        while (current)
        {
            if (current->faculty.id == id)
            {
                return &current->faculty; // Return pointer to the record
            }
            current = current->next;
        }
        return nullptr; // Not found
    }

    // Print the hash table
    void print()
    {
        for (int i = 0; i < size; ++i)
        {
            std::cout << "Bucket " << i << ": ";
            Node *current = table[i];
            while (current)
            {
                std::cout << "(ID: " << current->faculty.id
                          << ", Name: " << current->faculty.name
                          << ", Dept: " << current->faculty.department
                          << ", Exp: " << current->faculty.experience << " years) -> ";
                current = current->next;
            }
            std::cout << "nullptr" << std::endl;
        }
    }
};

// Example usage
int main()
{
    FacultyHashTable fht(10); // Hash table with 10 buckets

    // Insert faculty records
    fht.insert(101, "Dr. Alice Johnson", "Computer Science", 15);
    fht.insert(102, "Prof. Bob Smith", "Mathematics", 10);
    fht.insert(103, "Dr. Charlie Brown", "Physics", 20);
    fht.insert(111, "Prof. Diana Lee", "Chemistry", 12);    // 111 % 10 = 1, same as 101 % 10 = 1 -> collision, chains
    fht.insert(101, "Dr. Alice Johnson Updated", "AI", 16); // Replace existing ID 101

    // Print the table
    std::cout << "Faculty Hash Table after insertions:" << std::endl;
    fht.print();

    // Search for a faculty
    Faculty *found = fht.search(101);
    if (found)
    {
        std::cout << "\nFound Faculty: ID " << found->id
                  << ", Name: " << found->name
                  << ", Dept: " << found->department
                  << ", Exp: " << found->experience << " years" << std::endl;
    }
    else
    {
        std::cout << "\nFaculty not found." << std::endl;
    }

    // Search for a non-existent faculty
    found = fht.search(999);
    if (!found)
    {
        std::cout << "Faculty with ID 999 not found." << std::endl;
    }

    return 0;
}

```

## Output
```
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS57.exe'
Faculty Hash Table after insertions:
Bucket 0: nullptr
Bucket 1: (ID: 101, Name: Dr. Alice Johnson Updated, Dept: AI, Exp: 16 years) ->
(ID: 111, Name: Prof. Diana Lee, Dept: Chemistry, Exp: 12 years) -> nullptr
Bucket 2: (ID: 102, Name: Prof. Bob Smith, Dept: Mathematics, Exp: 10 years) ->
nullptr
Bucket 3: (ID: 103, Name: Dr. Charlie Brown, Dept: Physics, Exp: 20 years) ->
nullptr
Bucket 4: nullptr
Bucket 5: nullptr
Bucket 6: nullptr
Bucket 7: nullptr
Bucket 8: nullptr
Bucket 9: nullptr
Found Faculty: ID 101, Name: Dr. Alice Johnson Updated, Dept: AI, Exp: 16 years
Faculty with ID 999 not found

```

## Conclusion
This program demonstrates the implementation of a hash table for managing faculty records using the MOD hash function and chaining with replacement for collision resolution. The replacement mechanism ensures that only the latest record for a given ID is kept, which can be useful for updates. Chaining allows multiple records at the same index, providing flexibility in handling collisions. This approach is efficient for average-case operations and helps in understanding advanced collision handling techniques in hash tables.
