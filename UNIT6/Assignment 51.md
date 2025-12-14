# Assignment 51  

## Title  
**Implement a hash table with collision resolution using linear probing. Ass 1**  


---

## Theory  

Hash tables are fundamental data structures that store key-value pairs and enable efficient average-case time complexity for operations like insertion, deletion, and search, typically O(1). They utilize a hash function to map keys to indices in an underlying array, allowing direct access to stored elements.

A hash function is a mathematical function that computes an index (hash value) from a given key. Ideally, it distributes keys uniformly across the array to minimize collisions. However, collisions occur when two different keys produce the same hash index, which is inevitable due to the finite size of the array and the potentially infinite key space.

Collision resolution is crucial for handling such conflicts. Common techniques include separate chaining (using linked lists at each index) and open addressing (probing for alternative slots in the same array). Linear probing is a simple open addressing method where, upon encountering a collision, the algorithm linearly searches for the next available slot by incrementing the index (wrapping around using modulo operation) until an empty slot is found.

In this implementation, we build a hash table using linear probing for collision resolution. The table supports core operations: insert (adds a key if not present), search (finds the index of a key), and remove (marks a slot as deleted to maintain probing integrity). Deleted slots are marked separately to avoid breaking the probe sequence for subsequent searches.

**Advantages of Linear Probing:**  
- Simple to implement and understand.  
- Good cache performance due to contiguous memory access.  
- No additional data structures like linked lists are needed.

**Disadvantages:**  
- Primary clustering: Consecutive occupied slots can form clusters, increasing probe lengths and degrading performance.  
- Deletion handling requires careful marking to prevent search failures.  
- Worst-case time complexity can degrade to O(n) if the table becomes highly clustered.

This approach is suitable for scenarios where memory is constrained and simplicity is prioritized over advanced collision resolution methods like quadratic probing or double hashing.

---

## Code  
```cpp
#include <iostream>
#include <vector>
#include <utility> // for std::pair

using namespace std;

class HashTable
{
private:
    int size;
    vector<pair<int, int>> table; // pair<key, value>
    vector<int> status;           // 0: empty, 1: occupied, 2: deleted

    int hash(int key)
    {
        return key % size;
    }

public:
    HashTable(int s) : size(s), table(s), status(s, 0) {}

    void insert(int key, int value)
    {
        int index = hash(key);
        int start = index;
        while (status[index] == 1 && table[index].first != key)
        {
            index = (index + 1) % size;
            if (index == start)
            {
                cout << "Hash table is full!" << endl;
                return;
            }
        }
        if (status[index] == 1 && table[index].first == key)
        {
            table[index].second = value; // Update value
        }
        else
        {
            table[index] = {key, value};
            status[index] = 1;
        }
        cout << "Inserted (" << key << ", " << value << ") at index " << index << endl;
    }

    bool search(int key, int &value)
    {
        int index = hash(key);
        int start = index;
        while (status[index] != 0)
        {
            if (status[index] == 1 && table[index].first == key)
            {
                value = table[index].second;
                return true;
            }
            index = (index + 1) % size;
            if (index == start)
                break;
        }
        return false;
    }

    void remove(int key)
    {
        int index = hash(key);
        int start = index;
        while (status[index] != 0)
        {
            if (status[index] == 1 && table[index].first == key)
            {
                status[index] = 2; // Mark as deleted
                cout << "Deleted key " << key << " at index " << index << endl;
                return;
            }
            index = (index + 1) % size;
            if (index == start)
                break;
        }
        cout << "Key " << key << " not found!" << endl;
    }

    void display()
    {
        cout << "Hash Table:" << endl;
        for (int i = 0; i < size; i++)
        {
            if (status[i] == 1)
            {
                cout << "Index " << i << ": (" << table[i].first << ", " << table[i].second << ")" << endl;
            }
            else if (status[i] == 2)
            {
                cout << "Index " << i << ": Deleted" << endl;
            }
            else
            {
                cout << "Index " << i << ": Empty" << endl;
            }
        }
    }
};

int main()
{
    int size;
    cout << "Enter the size of the hash table: ";
    cin >> size;

    HashTable ht(size);

    int choice;
    do
    {
        cout << "\nMenu:\n";
        cout << "1. Insert\n";
        cout << "2. Search\n";
        cout << "3. Delete\n";
        cout << "4. Display\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
        {
            int key, value;
            cout << "Enter key and value: ";
            cin >> key >> value;
            ht.insert(key, value);
            break;
        }
        case 2:
        {
            int key, value;
            cout << "Enter key to search: ";
            cin >> key;
            if (ht.search(key, value))
            {
                cout << "Found: (" << key << ", " << value << ")" << endl;
            }
            else
            {
                cout << "Key not found!" << endl;
            }
            break;
        }
        case 3:
        {
            int key;
            cout << "Enter key to delete: ";
            cin >> key;
            ht.remove(key);
            break;
        }
        case 4:
            ht.display();
            break;
        case 5:
            cout << "Exiting..." << endl;
            break;
        default:
            cout << "Invalid choice!" << endl;
        }
    } while (choice != 5);

    return 0;
}


```
---
## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS51.exe'
Enter the size of the hash table: 5
Menu:
1. Insert
2. Search
3. Delete
4. Display
5. Exit
Enter your choice: 1
Enter key and value: 12 100
Inserted (12, 100) at index 2

Menu:
1. Insert
2. Search
3. Delete
4. Display
5. Exit
Enter your choice: 1
Enter key and value: 7 50
Inserted (7, 50) at index 3

Menu:
1. Insert
2. Search
3. Delete
4. Display
5. Exit
Enter your choice: 4
Hash Table:
Index 0: Empty
Index 1: Empty
Index 2: (12, 100)
Index 3: (7, 50)
Index 4: Empty

Menu:
1. Insert
2. Search
3. Delete
4. Display
5. Exit
Enter your choice: 2
Enter key to search: 12
Found: (12, 100)

Menu:
1. Insert
2. Search
3. Delete
4. Display
5. Exit
Enter your choice: 3
Enter key to delete: 12
Deleted key 12 at index 2

Menu:
1. Insert
2. Search
3. Delete
4. Display
5. Exit
Enter your choice: 5
Exiting...

---
## Conclusion  

In this program, we implemented a hash table with collision resolution using linear probing in C++. The implementation demonstrates efficient insertion, search, and deletion operations while handling collisions through linear probing. This approach provides a balance between simplicity and performance, making it suitable for various applications requiring fast key-based access. The use of linear probing ensures that the hash table remains compact and cache-friendly, though it may suffer from clustering in high-load scenarios.
