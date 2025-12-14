# Assignment 54  

## Title  
**Store and retrieve student records using roll numbers. Ass 4**  


---

## Theory  
Hash tables are ideal for storing and retrieving records efficiently when a unique key is available, such as student roll numbers. In this implementation, student records are stored in a hash table using separate chaining for collision resolution. Each record is a struct containing roll number (key), name, and marks.

The roll number serves as the hash key, computed using modulo operation to map to an index in the table. Collisions are handled by chaining records in linked lists at each index. This allows multiple students to share the same hash index without data loss.

**Key Operations:**  
- **Insertion:** Compute hash index from roll number, append the student record to the list at that index.  
- **Retrieval (Search):** Compute hash index, traverse the list to find the matching roll number, and display the record if found.  

**Advantages:**  
- Fast average-case lookup (O(1)) for retrieval by roll number.  
- Handles dynamic data with varying numbers of records.  
- Suitable for applications like student management systems where quick access by ID is required.  

**Disadvantages:**  
- Worst-case performance can degrade to O(n) if many collisions occur.  
- Memory overhead for linked lists.  
- Relies on a good hash function to distribute keys evenly.  

This approach demonstrates practical use of hash tables in record management, emphasizing key-based access and collision handling.

---

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
    float grade;

    // Constructor
    Student(int roll, std::string n, float g) : rollNumber(roll), name(n), grade(g) {}
};

// Node structure for the linked list
struct Node
{
    Student student;
    Node *next;

    // Constructor
    Node(Student s) : student(s), next(nullptr) {}
};

// HashTable class for student records
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
    void insert(int rollNumber, const std::string &name, float grade)
    {
        int index = hashFunction(rollNumber);
        Student newStudent(rollNumber, name, grade);
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
            current->student.grade = grade;
            delete newNode;
            return;
        }
        // Append new node
        current->next = newNode;
    }

    // Search for a student by roll number and return the record (or nullptr if not found)
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

    // Print the hash table (for debugging)
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
    sht.insert(101, "Alice", 85.5);
    sht.insert(102, "Bob", 92.0);
    sht.insert(103, "Charlie", 78.3);
    sht.insert(111, "Diana", 88.7); // Might collide with others

    // Print the table
    std::cout << "Student Hash Table after insertions:" << std::endl;
    sht.print();

    // Search for a student
    Student *found = sht.search(102);
    if (found)
    {
        std::cout << "\nFound Student: Roll " << found->rollNumber
                  << ", Name: " << found->name
                  << ", Grade: " << found->grade << std::endl;
    }
    else
    {
        std::cout << "\nStudent not found." << std::endl;
    }

    // Search for a non-existent student
    found = sht.search(999);
    if (!found)
    {
        std::cout << "Student with roll 999 not found." << std::endl;
    }

    // Remove a student
    sht.remove(102);
    std::cout << "\nAfter removing roll 102:" << std::endl;
    sht.print();

    return 0;
}

```
---
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS54.exe'
Student Hash Table after insertions:
Bucket 0: nullptr
Bucket 1: (Roll: 101, Name: Alice, Grade: 85.5) -> (Roll: 111, Name: Diana,
Grade: 88.7) -> nullptr
Bucket 2: (Roll: 102, Name: Bob, Grade: 92) -> nullptr
Bucket 3: (Roll: 103, Name: Charlie, Grade: 78.3) -> nullptr
Bucket 4: nullptr
Bucket 5: nullptr
Bucket 6: nullptr
Bucket 7: nullptr
Bucket 8: nullptr
Bucket 9: nullptr
Found Student: Roll 102, Name: Bob, Grade: 92
Student with roll 999 not found.
After removing roll 102:
Bucket 0: nullptr
Bucket 1: (Roll: 101, Name: Alice, Grade: 85.5) -> (Roll: 111, Name: Diana,
Grade: 88.7) -> nullptr
Bucket 2: nullptr
Bucket 3: (Roll: 103, Name: Charlie, Grade: 78.3) -> nullptr
Bucket 4: nullptr
Bucket 5: nullptr
Bucket 6: nullptr
Bucket 7: nullptr
Bucket 8: nullptr
Bucket 9: nullptr

---
## Conclusion  
In this program, we implemented a hash table to store and retrieve student records using roll numbers as keys in C++. Separate chaining handles collisions, allowing efficient insertion and search operations. The system enables quick access to student details by roll number, demonstrating the practical application of hash tables in data management. This approach is scalable for larger datasets and provides a foundation for more complex record-keeping systems.
