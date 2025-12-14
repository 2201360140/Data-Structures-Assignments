# Assignment 60

## Title
Design and implement a smart college placement portal that uses advanced hashing techniques to efficiently manage student placement records with high performance and low collision probability, even under dynamic data growth.

## Theory
A hash table is a data structure that provides efficient insertion, search, and deletion operations using a hash function to map keys to indices in an array. For a smart college placement portal, we use separate chaining with a vector of linked lists to handle collisions, ensuring low collision probability and high performance.

Separate chaining uses a list at each index to store multiple records that hash to the same bucket, avoiding clustering issues in open addressing. The hash function is key % capacity, where capacity is the number of buckets (e.g., 11).

To handle dynamic data growth, the table can be resized by increasing the number of buckets and rehashing all records, maintaining load factor below a threshold (e.g., 0.75). This is not implemented in the code but can be added for scalability.

### Example of Separate Chaining
Suppose capacity = 11, insert ID 301.
- Hash: 301 % 11 = 8, insert in bucket 8.
- If another ID hashes to 8, add to the list in bucket 8.

### Diagramatic Representation
```
Hash Table (Capacity 11):
Bucket 0: [Record1] -> [Record2]
Bucket 1: nullptr
Bucket 2: [Record3]
...
Bucket 8: [Record for ID 301]
...
```
This design ensures efficient management of placement records with minimal collisions.

## Code
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <functional> // For std::hash

// Placement record structure
struct PlacementRecord
{
    int studentID;
    std::string name;
    std::string company;
    std::string role;
    float salary;
    std::string placementDate;

    // Constructor
    PlacementRecord(int id = -1, std::string n = "", std::string c = "", std::string r = "", float s = 0.0, std::string d = "")
        : studentID(id), name(n), company(c), role(r), salary(s), placementDate(d) {}
};

// HashTable class using double hashing with dynamic resizing
class PlacementHashTable
{
private:
    std::vector<PlacementRecord *> table;      // Vector of pointers to records
    int size;                                  // Current table size (prime number)
    int count;                                 // Number of records
    const double LOAD_FACTOR_THRESHOLD = 0.75; // Resize when exceeded

    // Primary hash function
    int primaryHash(int studentID)
    {
        return std::hash<int>{}(studentID) % size;
    }

    // Secondary hash function (for double hashing)
    int secondaryHash(int studentID)
    {
        return 1 + (std::hash<int>{}(studentID) % (size - 1));
    }

    // Check if a number is prime
    bool isPrime(int n)
    {
        if (n <= 1)
            return false;
        for (int i = 2; i * i <= n; ++i)
        {
            if (n % i == 0)
                return false;
        }
        return true;
    }

    // Get next prime size
    int nextPrime(int n)
    {
        int candidate = n + 1;
        while (!isPrime(candidate))
        {
            candidate++;
        }
        return candidate;
    }

    // Resize the table
    void resize()
    {
        int newSize = nextPrime(size * 2);
        std::vector<PlacementRecord *> newTable(newSize, nullptr);

        // Rehash all existing records
        for (int i = 0; i < size; ++i)
        {
            if (table[i])
            {
                int id = table[i]->studentID;
                int index = std::hash<int>{}(id) % newSize;
                int step = 1 + (std::hash<int>{}(id) % (newSize - 1));
                int j = 0;
                while (newTable[(index + j * step) % newSize] != nullptr)
                {
                    j++;
                }
                newTable[(index + j * step) % newSize] = table[i];
            }
        }

        table = std::move(newTable);
        size = newSize;
    }

public:
    // Constructor: Initialize with a prime size
    PlacementHashTable(int initialSize = 11) : size(initialSize), count(0)
    {
        if (!isPrime(size))
            size = nextPrime(size);
        table.resize(size, nullptr);
    }

    // Destructor: Clean up all dynamically allocated records
    ~PlacementHashTable()
    {
        for (auto &record : table)
        {
            if (record)
                delete record;
        }
    }

    // Insert a placement record
    void insert(int studentID, const std::string &name, const std::string &company, const std::string &role, float salary, const std::string &placementDate)
    {
        if ((double)count / size > LOAD_FACTOR_THRESHOLD)
        {
            resize();
        }

        int index = primaryHash(studentID);
        int step = secondaryHash(studentID);
        int i = 0;

        // Double hashing probe
        while (table[(index + i * step) % size] != nullptr && table[(index + i * step) % size]->studentID != studentID)
        {
            i++;
            if (i >= size)
            {
                std::cout << "Table full after probing!" << std::endl;
                return;
            }
        }

        int finalIndex = (index + i * step) % size;
        if (table[finalIndex] != nullptr && table[finalIndex]->studentID == studentID)
        {
            // Update existing record
            table[finalIndex]->name = name;
            table[finalIndex]->company = company;
            table[finalIndex]->role = role;
            table[finalIndex]->salary = salary;
            table[finalIndex]->placementDate = placementDate;
        }
        else
        {
            // Insert new record
            table[finalIndex] = new PlacementRecord(studentID, name, company, role, salary, placementDate);
            count++;
        }
    }

    // Search for a placement record by student ID
    PlacementRecord *search(int studentID)
    {
        int index = primaryHash(studentID);
        int step = secondaryHash(studentID);
        int i = 0;

        // Double hashing probe
        while (table[(index + i * step) % size] != nullptr)
        {
            if (table[(index + i * step) % size]->studentID == studentID)
            {
                return table[(index + i * step) % size];
            }
            i++;
            if (i >= size)
                break;
        }
        return nullptr; // Not found
    }

    // Delete a placement record by student ID
    void remove(int studentID)
    {
        int index = primaryHash(studentID);
        int step = secondaryHash(studentID);
        int i = 0;

        // Double hashing probe
        while (table[(index + i * step) % size] != nullptr)
        {
            if (table[(index + i * step) % size]->studentID == studentID)
            {
                delete table[(index + i * step) % size];
                table[(index + i * step) % size] = nullptr;
                count--;
                return;
            }
            i++;
            if (i >= size)
                break;
        }
        // Not found, do nothing
    }

    // Print the hash table
    void print()
    {
        std::cout << "Placement Hash Table (Size: " << size << ", Records: " << count << "):" << std::endl;
        for (int i = 0; i < size; ++i)
        {
            std::cout << "Slot " << i << ": ";
            if (table[i])
            {
                std::cout << "(ID: " << table[i]->studentID
                          << ", Name: " << table[i]->name
                          << ", Company: " << table[i]->company
                          << ", Role: " << table[i]->role
                          << ", Salary: $" << table[i]->salary
                          << ", Date: " << table[i]->placementDate << ")";
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
    PlacementHashTable pht; // Starts with size 11

    // Insert placement records
    pht.insert(101, "Alice Johnson", "Google", "Software Engineer", 120000, "2023-06-15");
    pht.insert(102, "Bob Smith", "Microsoft", "Data Analyst", 95000, "2023-07-20");
    pht.insert(103, "Charlie Brown", "Amazon", "Product Manager", 110000, "2023-08-10");
    pht.insert(104, "Diana Lee", "Tesla", "AI Researcher", 130000, "2023-09-05");
    pht.insert(105, "Eve Wilson", "Apple", "UX Designer", 100000, "2023-10-01");
    // This may trigger resize if load factor > 0.75

    // Print the table
    pht.print();

    // Search for a record
    PlacementRecord *found = pht.search(102);
    if (found)
    {
        std::cout << "\nFound Placement: ID " << found->studentID
                  << ", Name: " << found->name
                  << ", Company: " << found->company
                  << ", Role: " << found->role
                  << ", Salary: $" << found->salary
                  << ", Date: " << found->placementDate << std::endl;
    }
    else
    {
        std::cout << "\nPlacement not found." << std::endl;
    }

    // Delete a record
    pht.remove(103);
    std::cout << "\nAfter removing ID 103:" << std::endl;
    pht.print();

    // Search for a non-existent record
    found = pht.search(999);
    if (!found)
    {
        std::cout << "\nPlacement with ID 999 not found." << std::endl;
    }

    return 0;
}

```

## Output
```
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS60.exe'
Placement Hash Table (Size: 11, Records: 5):
Slot 0: Empty
Slot 1: Empty
Slot 2: (ID: 101, Name: Alice Johnson, Company: Google, Role: Software Engineer,
Salary: $120000, Date: 2023-06-15)
Slot 3: (ID: 102, Name: Bob Smith, Company: Microsoft, Role: Data Analyst,
Salary: $95000, Date: 2023-07-20)
Slot 4: (ID: 103, Name: Charlie Brown, Company: Amazon, Role: Product Manager,
Salary: $110000, Date: 2023-08-10)
Slot 5: (ID: 104, Name: Diana Lee, Company: Tesla, Role: AI Researcher, Salary:
$130000, Date: 2023-09-05)
Slot 6: (ID: 105, Name: Eve Wilson, Company: Apple, Role: UX Designer, Salary:
$100000, Date: 2023-10-01)
Slot 7: Empty
Slot 8: Empty
Slot 9: Empty
Slot 10: Empty
Found Placement: ID 102, Name: Bob Smith, Company: Microsoft, Role: Data Analyst,
Salary: $95000, Date: 2023-07-20
After removing ID 103:
Placement Hash Table (Size: 11, Records: 4):
Slot 0: Empty
Slot 1: Empty
Slot 2: (ID: 101, Name: Alice Johnson, Company: Google, Role: Software Engineer,
Salary: $120000, Date: 2023-06-15)
Slot 3: (ID: 102, Name: Bob Smith, Company: Microsoft, Role: Data Analyst,
Salary: $95000, Date: 2023-07-20)
Slot 4: Empty
Slot 5: (ID: 104, Name: Diana Lee, Company: Tesla, Role: AI Researcher, Salary:
$130000, Date: 2023-09-05)
Slot 6: (ID: 105, Name: Eve Wilson, Company: Apple, Role: UX Designer, Salary:
$100000, Date: 2023-10-01)
Slot 7: Empty
Slot 8: Empty
Slot 9: Empty
Slot 10: Empty
Placement with ID 999 not found.

```

## Conclusion
This program implements a smart college placement portal using a hash table with separate chaining for collision handling. It efficiently manages student placement records with operations for insertion, search, and deletion. The design ensures low collision probability through separate chaining and can be extended with dynamic resizing for handling data growth, providing high performance for a placement portal.
