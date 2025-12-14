# Assignment 55

## Title
WAP to simulate a faculty database as a hash table. Search a particular faculty by using MOD as a hash function for linear probing method of collision handling technique. Assume suitable data for faculty record.

## Theory
A hash table is a data structure that implements an associative array abstract data type, a structure that can map keys to values. It uses a hash function to compute an index into an array of buckets or slots, from which the desired value can be found.

In this program, we simulate a faculty database using a hash table. Each faculty record consists of an ID (integer), name (string), and department (string). The hash function used is the MOD operation: hash(key) = key % size, where size is the table size (10 in this case).

For collision handling, we use linear probing. When a collision occurs (two keys hash to the same index), we probe the next slot in the table (index + 1) % size until an empty slot is found for insertion or the key is found for search. This method is simple but can lead to clustering.

### Example of Hash Function and Linear Probing
Suppose we have a hash table of size 10, and we want to insert faculty with ID 101.
- Hash function: 101 % 10 = 1, so index 1.
- If index 1 is empty, insert there.
- If occupied, probe to index 2, then 3, etc., until an empty slot is found.

### Diagramatic Representation
```
Hash Table (Size 10):
Index: 0  1  2  3  4  5  6  7  8  9
Value: -  F1 -  F2 F3 -  -  -  -  -
```
Where F1, F2, F3 are faculty records at their hashed or probed positions.

The program allows inserting faculty records into the hash table and searching for a faculty by ID. If the faculty is found, their details are displayed; otherwise, a "not found" message is shown.

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

// HashTable class using linear probing
class FacultyHashTable
{
private:
    std::vector<Faculty *> table; // Vector of pointers to Faculty records
    int size;                     // Number of slots in the table
    int count;                    // Number of records inserted

    // Hash function: ID modulo size
    int hashFunction(int id)
    {
        return id % size;
    }

public:
    // Constructor: Initialize table with given size
    FacultyHashTable(int s) : size(s), count(0)
    {
        table.resize(size, nullptr);
    }

    // Destructor: Clean up all dynamically allocated records
    ~FacultyHashTable()
    {
        for (int i = 0; i < size; ++i)
        {
            if (table[i])
            {
                delete table[i];
            }
        }
    }

    // Insert a faculty record
    bool insert(int id, const std::string &name, const std::string &department, int experience)
    {
        if (count >= size)
        {
            std::cout << "Hash table is full!" << std::endl;
            return false;
        }

        int index = hashFunction(id);
        int originalIndex = index;

        // Linear probing to find an empty slot or existing ID
        while (table[index] != nullptr && table[index]->id != id)
        {
            index = (index + 1) % size;
            if (index == originalIndex)
            {
                std::cout << "Hash table is full (wrapped around)!" << std::endl;
                return false;
            }
        }

        // If ID already exists, update the record
        if (table[index] != nullptr && table[index]->id == id)
        {
            table[index]->name = name;
            table[index]->department = department;
            table[index]->experience = experience;
        }
        else
        {
            // Insert new record
            table[index] = new Faculty(id, name, department, experience);
            count++;
        }
        return true;
    }

    // Search for a faculty by ID
    Faculty *search(int id)
    {
        int index = hashFunction(id);
        int originalIndex = index;

        // Linear probing to find the ID
        while (table[index] != nullptr)
        {
            if (table[index]->id == id)
            {
                return table[index];
            }
            index = (index + 1) % size;
            if (index == originalIndex)
            {
                break; // Wrapped around, not found
            }
        }
        return nullptr; // Not found
    }

    // Print the hash table
    void print()
    {
        for (int i = 0; i < size; ++i)
        {
            std::cout << "Slot " << i << ": ";
            if (table[i])
            {
                std::cout << "(ID: " << table[i]->id
                          << ", Name: " << table[i]->name
                          << ", Dept: " << table[i]->department
                          << ", Exp: " << table[i]->experience << " years)";
            }
            else
            {
                std::cout << "Empty";
            }
            std::cout << std::endl;
        }
    }
};

// Example usage
int main()
{
    FacultyHashTable fht(10); // Hash table with 10 slots

    // Insert faculty records
    fht.insert(101, "Dr. Alice Johnson", "Computer Science", 15);
    fht.insert(102, "Prof. Bob Smith", "Mathematics", 10);
    fht.insert(103, "Dr. Charlie Brown", "Physics", 20);
    fht.insert(111, "Prof. Diana Lee", "Chemistry", 12); // 111 % 10 = 1, same as 101 % 10 = 1 -> collision, probes to next slot

    // Print the table
    std::cout << "Faculty Hash Table after insertions:" << std::endl;
    fht.print();

    // Search for a faculty
    Faculty *found = fht.search(102);
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
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS55.exe'
Faculty Hash Table after insertions:
Slot 0: Empty
Slot 1: (ID: 101, Name: Dr. Alice Johnson, Dept: Computer Science, Exp: 15 years)
Slot 2: (ID: 102, Name: Prof. Bob Smith, Dept: Mathematics, Exp: 10 years)
Slot 3: (ID: 103, Name: Dr. Charlie Brown, Dept: Physics, Exp: 20 years)
Slot 4: (ID: 111, Name: Prof. Diana Lee, Dept: Chemistry, Exp: 12 years)
Slot 5: Empty
Slot 6: Empty
Slot 7: Empty
Slot 8: Empty
Slot 9: Empty
Found Faculty: ID 102, Name: Prof. Bob Smith, Dept: Mathematics, Exp: 10 years
Faculty with ID 999 not found.


```

## Conclusion
This program demonstrates the implementation of a hash table for managing faculty records, utilizing the MOD hash function and linear probing for collision resolution. It efficiently handles insertion and search operations, providing a basic database simulation. The use of linear probing ensures that collisions are resolved by finding the next available slot, though it may lead to performance issues in high-load scenarios due to clustering. This approach is suitable for small-scale applications and serves as a foundation for understanding hash table concepts in data structures.
