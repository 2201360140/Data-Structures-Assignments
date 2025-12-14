# Assignment 59

## Title
WAP to simulate student databases as a hash table. a student database management system using hashing techniques to allow efficient insertion, search, and deletion of student records. Give the code

## Theory
A hash table is a data structure that provides efficient insertion, search, and deletion operations using a hash function to map keys to indices in an array. This program simulates a student database management system using a hash table with the MOD hash function (key % size) and linear probing for collision handling.

Linear probing resolves collisions by placing the colliding element in the next available slot in the array. The hash table supports:
- Insertion: Add a student record, probing for an empty slot if collision occurs.
- Search: Find a student by ID, probing until found or table end.
- Deletion: Remove a student by ID, setting the slot to nullptr.

### Example of MOD Hash Function and Linear Probing
Suppose table size = 10, insert student ID 201.
- Hash: 201 % 10 = 1, insert at index 1.
- If index 1 occupied, probe to 2, 3, etc.

### Diagramatic Representation
```
Hash Table (Size 10):
Index: 0 -> nullptr
       1 -> Student1
       2 -> Student2 (after probing)
       3 -> nullptr
       ...
```
Operations demonstrate efficient management of student records.

## Code
```cpp
#include <iostream>
#include <vector>
#include <string>

// Student record structure
struct Student
{
    int rollNumber;
    std::string name;
    int age;
    float grade;

    // Constructor
    Student(int roll = -1, std::string n = "", int a = 0, float g = 0.0)
        : rollNumber(roll), name(n), age(a), grade(g) {}
};

// Node structure for the linked list
struct Node
{
    Student student;
    Node *next;

    // Constructor
    Node(Student s) : student(s), next(nullptr) {}
};

// HashTable class for student database
class StudentHashTable
{
private:
    std::vector<Node *> table; // Vector of pointers to linked lists
    int size;                  // Number of buckets

    // Hash function: Roll number modulo size
    int hashFunction(int rollNumber)
    {
        return rollNumber % size;
    }

public:
    // Constructor: Initialize table with given size
    StudentHashTable(int s) : size(s)
    {
        table.resize(size, nullptr);
    }

    // Destructor: Clean up all dynamically allocated nodes
    ~StudentHashTable()
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

    // Insert a student record
    void insert(int rollNumber, const std::string &name, int age, float grade)
    {
        int index = hashFunction(rollNumber);
        Student newStudent(rollNumber, name, age, grade);
        Node *newNode = new Node(newStudent); // Dynamic allocation

        // If bucket is empty, set as head
        if (!table[index])
        {
            table[index] = newNode;
            return;
        }

        // Traverse to the end of the list and append (or update if roll number exists)
        Node *current = table[index];
        while (current->next)
        {
            if (current->student.rollNumber == rollNumber)
            {
                // Update existing record
                current->student.name = name;
                current->student.age = age;
                current->student.grade = grade;
                delete newNode; // No need for new node
                return;
            }
            current = current->next;
        }
        // Check the last node
        if (current->student.rollNumber == rollNumber)
        {
            current->student.name = name;
            current->student.age = age;
            current->student.grade = grade;
            delete newNode;
            return;
        }
        // Append new node
        current->next = newNode;
    }

    // Search for a student by roll number
    Student *search(int rollNumber)
    {
        int index = hashFunction(rollNumber);
        Node *current = table[index];
        while (current)
        {
            if (current->student.rollNumber == rollNumber)
            {
                return &current->student; // Return pointer to the record
            }
            current = current->next;
        }
        return nullptr; // Not found
    }

    // Delete a student record by roll number
    void remove(int rollNumber)
    {
        int index = hashFunction(rollNumber);
        Node *current = table[index];
        Node *prev = nullptr;

        while (current)
        {
            if (current->student.rollNumber == rollNumber)
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
        // Roll number not found, do nothing
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
                std::cout << "(Roll: " << current->student.rollNumber
                          << ", Name: " << current->student.name
                          << ", Age: " << current->student.age
                          << ", Grade: " << current->student.grade << ") -> ";
                current = current->next;
            }
            std::cout << "nullptr" << std::endl;
        }
    }
};

// Example usage
int main()
{
    StudentHashTable sht(10); // Hash table with 10 buckets

    // Insert student records
    sht.insert(101, "Alice", 20, 85.5);
    sht.insert(102, "Bob", 21, 92.0);
    sht.insert(103, "Charlie", 19, 78.3);
    sht.insert(111, "Diana", 22, 88.7); // 111 % 10 = 1, same as 101 % 10 = 1 -> collision

    // Print the table
    std::cout << "Student Hash Table after insertions:" << std::endl;
    sht.print();

    // Search for a student
    Student *found = sht.search(102);
    if (found)
    {
        std::cout << "\nFound Student: Roll " << found->rollNumber
                  << ", Name: " << found->name
                  << ", Age: " << found->age
                  << ", Grade: " << found->grade << std::endl;
    }
    else
    {
        std::cout << "\nStudent not found." << std::endl;
    }

    // Delete a student
    sht.remove(102);
    std::cout << "\nAfter removing roll 102:" << std::endl;
    sht.print();

    // Search for a non-existent student
    found = sht.search(999);
    if (!found)
    {
        std::cout << "\nStudent with roll 999 not found." << std::endl;
    }

    return 0;
}

```

## Output
```
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS59.exe'
Student Hash Table after insertions:
Bucket 0: nullptr
Bucket 1: (Roll: 101, Name: Alice, Age: 20, Grade: 85.5) -> (Roll: 111, Name:
Diana, Age: 22, Grade: 88.7) -> nullptr
Bucket 2: (Roll: 102, Name: Bob, Age: 21, Grade: 92) -> nullptr
Bucket 3: (Roll: 103, Name: Charlie, Age: 19, Grade: 78.3) -> nullptr
Bucket 4: nullptr
Bucket 5: nullptr
Bucket 6: nullptr
Bucket 7: nullptr
Bucket 8: nullptr
Bucket 9: nullptr
Found Student: Roll 102, Name: Bob, Age: 21, Grade: 92
After removing roll 102:
Bucket 0: nullptr
Bucket 1: (Roll: 101, Name: Alice, Age: 20, Grade: 85.5) -> (Roll: 111, Name:
Diana, Age: 22, Grade: 88.7) -> nullptr
Bucket 2: nullptr
Bucket 3: (Roll: 103, Name: Charlie, Age: 19, Grade: 78.3) -> nullptr
Bucket 4: nullptr
Bucket 5: nullptr
Bucket 6: nullptr
Bucket 7: nullptr
Bucket 8: nullptr
Bucket 9: nullptr
Student with roll 999 not found.

```

## Conclusion
This program demonstrates a student database management system using a hash table with MOD hashing and linear probing. It efficiently handles insertion, search, and deletion of student records, showcasing the practical application of hashing techniques in data management. The linear probing method ensures collision resolution, making the system robust for average-case operations.
