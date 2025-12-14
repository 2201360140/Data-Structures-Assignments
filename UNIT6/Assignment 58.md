# Assignment 58

## Title
WAP to simulate employee databases as a hash table. Search a particular faculty by using Mid square method as a hash function for linear probing method of collision handling technique. Assume suitable data for faculty record.

## Theory
A hash table is a data structure that maps keys to values using a hash function. The mid-square method is a hashing technique where the key is squared, and the middle digits of the result are used as the hash value.

In this program, we simulate a faculty database using a hash table with linear probing for collision handling. The hash function uses the mid-square method: square the key, convert to string, take the middle digit, and mod by table size.

Linear probing resolves collisions by checking the next slot in the array until an empty slot is found for insertion or the key is found for search.

### Example of Mid-Square Hash Function
Suppose key = 101, table size = 10.
- Square: 101 * 101 = 10201
- String: "10201", length 5, middle index 2, digit '2'
- Hash: 2 % 10 = 2

### Diagramatic Representation
```
Hash Table (Size 10):
Index: 0 -> nullptr
       1 -> Faculty1
       2 -> Faculty2 (after mid-square)
       3 -> nullptr
       ...
```
Linear probing: If index 2 is occupied, probe to 3, 4, etc.

The program inserts faculty records and searches by ID using this method.

## Code
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cmath> // For pow

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

// HashTable class using Mid Square hash function and linear probing
class FacultyHashTable
{
private:
    std::vector<Faculty *> table; // Vector of pointers to Faculty records
    int size;                     // Number of slots in the table
    int count;                    // Number of records inserted

    // Mid Square hash function
    int hashFunction(int id)
    {
        // Assume id is a 3-digit number for simplicity (e.g., 123)
        long long squared = (long long)id * id; // Square the id
        std::string squaredStr = std::to_string(squared);

        // Extract middle digits (for 3-digit id, squared is 5-6 digits, take middle 3)
        int len = squaredStr.length();
        int start = (len - 3) / 2; // Start index for middle 3 digits
        std::string middleStr = squaredStr.substr(start, 3);
        int middle = std::stoi(middleStr);

        return middle % size;
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

    // Insert faculty records (assuming 3-digit IDs for Mid Square)
    fht.insert(123, "Dr. Alice Johnson", "Computer Science", 15); // 123^2 = 15129, middle 512, 512 % 10 = 2
    fht.insert(456, "Prof. Bob Smith", "Mathematics", 10);        // 456^2 = 207936, middle 079, 79 % 10 = 9
    fht.insert(789, "Dr. Charlie Brown", "Physics", 20);          // 789^2 = 622521, middle 225, 225 % 10 = 5
    fht.insert(234, "Prof. Diana Lee", "Chemistry", 12);          // 234^2 = 54756, middle 475, 475 % 10 = 5 -> collision with 789, probes to next

    // Print the table
    std::cout << "Faculty Hash Table after insertions:" << std::endl;
    fht.print();

    // Search for a faculty
    Faculty *found = fht.search(456);
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
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS58.exe'
Faculty Hash Table after insertions:
Slot 0: Empty
Slot 1: Empty
Slot 2: (ID: 123, Name: Dr. Alice Johnson, Dept: Computer Science, Exp: 15 years)
Slot 3: Empty
Slot 4: Empty
Slot 5: (ID: 789, Name: Dr. Charlie Brown, Dept: Physics, Exp: 20 years)
Slot 6: (ID: 234, Name: Prof. Diana Lee, Dept: Chemistry, Exp: 12 years)
Slot 7: Empty
Slot 8: Empty
Slot 9: (ID: 456, Name: Prof. Bob Smith, Dept: Mathematics, Exp: 10 years)
Found Faculty: ID 456, Name: Prof. Bob Smith, Dept: Mathematics, Exp: 10 years
Faculty with ID 999 not found.

```

## Conclusion
This program demonstrates the mid-square hash function with linear probing for collision resolution in a hash table simulating a faculty database. The mid-square method provides a simple way to generate hash values, and linear probing ensures efficient handling of collisions. The implementation allows for insertion and search operations, showcasing the practical application of these hashing techniques in data structures.
