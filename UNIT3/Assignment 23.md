# **Assignment 23**

## **Title**

Write a program to implement multiple stack i.e more than two stack using array and perform following operations on it:
- A. Push
- B. Pop
- C. Stack Overflow
- D. Stack Underflow
- E. Display

## **Theory**

Multiple stacks can be efficiently implemented in a single array by dividing the array into equal segments, where each segment represents one stack. This approach optimizes memory usage compared to creating separate arrays for each stack. The implementation uses linked list approach where each stack maintains its own top pointer and grows independently within its allocated space.

**Key Concepts:**

1. **Array Division Strategy**:
   - Divide array into k equal parts for k stacks
   - Each stack gets size = total_size / number_of_stacks
   - Stack i occupies indices from (i * stack_size) to ((i+1) * stack_size - 1)

2. **Top Pointer Management**:
   - Each stack maintains its own top index
   - Top is initialized to -1 (relative to stack's base) indicating empty stack
   - Top increments on push and decrements on pop

3. **Operations**:
   - **Push**: Add element at top+1 position if space available
   - **Pop**: Remove element from top position if stack not empty
   - **Overflow**: Occurs when top reaches maximum size for that stack
   - **Underflow**: Occurs when attempting to pop from empty stack
   - **Display**: Show all elements in a specific stack

4. **Index Calculation**:
   - Actual array index = (stack_number * stack_size) + relative_top
   - This formula maps logical stack position to physical array location

**Algorithm Steps:**

**Push Operation:**
1. Check if stack top has reached its maximum size
2. If yes, report overflow error
3. If no, increment top and insert element at calculated index
4. Display success message

**Pop Operation:**
1. Check if stack is empty (top == -1)
2. If yes, report underflow error
3. If no, retrieve element at top position
4. Decrement top and return the element

**Display Operation:**
1. Check if stack is empty
2. If yes, display "Stack is empty" message
3. If no, traverse from base to top and print all elements

## **Example Execution**

Let's demonstrate with 3 stacks, each of size 3:

**Initial State:**
- Array size: 9 (3 stacks × 3 size)
- Stack 0: indices 0-2
- Stack 1: indices 3-5
- Stack 2: indices 6-8
- All tops: -1 (empty)

**Operations:**

1. **Push 10 to Stack 0**
   - top[0] becomes 0
   - arr[0] = 10
   - Output: "Pushed 10 to Stack 0"

2. **Push 20 to Stack 0**
   - top[0] becomes 1
   - arr[1] = 20
   - Output: "Pushed 20 to Stack 0"

3. **Push 30 to Stack 1**
   - top[1] becomes 0
   - arr[3] = 30
   - Output: "Pushed 30 to Stack 1"

4. **Push 40 to Stack 0**
   - top[0] becomes 2
   - arr[2] = 40
   - Output: "Pushed 40 to Stack 0"

5. **Push 50 to Stack 0** (Overflow)
   - top[0] is already 2 (max)
   - Output: "Stack Overflow! Stack 0 is full."

6. **Pop from Stack 0**
   - Returns 40
   - top[0] becomes 1
   - Output: "Popped 40 from Stack 0"

7. **Pop from Stack 2** (Underflow)
   - top[2] is -1 (empty)
   - Output: "Stack Underflow! Stack 2 is empty."

8. **Display Stack 0**
   - Output: "Stack 0 elements (bottom to top): 10 20"

**Final Array State:**
- Indices 0-1: 10, 20 (Stack 0)
- Index 3: 30 (Stack 1)
- Other indices: unused
---
## **Code**

```cpp
#include <iostream>
using namespace std;

#define MAX 100

class MultiStack
{
    int arr[MAX];  
    int top[10];   
    int start[10]; 
    int end[10];   
    int n, size;   

public:
    MultiStack(int stacks, int totalSize)
    {
        n = stacks;
        size = totalSize / n;

        
        for (int i = 0; i < n; i++)
        {
            start[i] = i * size;
            end[i] = start[i] + size - 1;
            top[i] = start[i] - 1; 
        }
    }

    void push(int stackNum, int value)
    {
        if (top[stackNum] == end[stackNum])
        {
            cout << "Stack " << stackNum + 1 << " Overflow!\n";
            return;
        }
        arr[++top[stackNum]] = value;
        cout << "Pushed " << value << " to Stack " << stackNum + 1 << endl;
    }

    void pop(int stackNum)
    {
        if (top[stackNum] < start[stackNum])
        {
            cout << "Stack " << stackNum + 1 << " Underflow!\n";
            return;
        }
        cout << "Popped " << arr[top[stackNum]--] << " from Stack " << stackNum + 1 << endl;
    }

    void display(int stackNum)
    {
        if (top[stackNum] < start[stackNum])
        {
            cout << "Stack " << stackNum + 1 << " is empty.\n";
            return;
        }
        cout << "Stack " << stackNum + 1 << " elements: ";
        for (int i = start[stackNum]; i <= top[stackNum]; i++)
            cout << arr[i] << " ";
        cout << endl;
    }
};

int main()
{
    int stacks, totalSize;
    cout << "Enter number of stacks (more than 2): ";
    cin >> stacks;
    cout << "Enter total array size: ";
    cin >> totalSize;

    MultiStack ms(stacks, totalSize);

    int choice, stackNum, value;
    do
    {
        cout << "\n--- MENU ---\n";
        cout << "1. Push\n2. Pop\n3. Display\n4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            cout << "Enter stack number (1-" << stacks << "): ";
            cin >> stackNum;
            cout << "Enter value: ";
            cin >> value;
            ms.push(stackNum - 1, value);
            break;

        case 2:
            cout << "Enter stack number (1-" << stacks << "): ";
            cin >> stackNum;
            ms.pop(stackNum - 1);
            break;

        case 3:
            cout << "Enter stack number (1-" << stacks << "): ";
            cin >> stackNum;
            ms.display(stackNum - 1);
            break;

        case 4:
            cout << "Exiting program...\n";
            break;

        default:
            cout << "Invalid choice!\n";
        }
    } while (choice != 4);

    return 0;
}

```
## **Output**

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS23.exe'
Enter number of stacks (more than 2): 3
Enter total array size: 9
--- MENU ---
1. Push
2. Pop
3. Display
4. Exit
Enter choice: 1
Enter stack number (1-3): 1
Enter value: 10
Pushed 10 to Stack 1
--- MENU ---
1. Push
2. Pop
3. Display
4. Exit
Enter choice: 1
Enter stack number (1-3): 2
Enter value: 10
Pushed 10 to Stack 2
--- MENU ---
1. Push
2. Pop
3. Display
4. Exit
Enter choice: 1
Enter stack number (1-3): 3
Enter value: 10
Pushed 10 to Stack 3
--- MENU ---
1. Push
2. Pop
3. Display
4. Exit
Enter choice: 3
Enter stack number (1-3): 1
Stack 1 elements: 10
--- MENU ---
1. Push
2. Pop
3. Display
4. Exit
Enter choice: 3
Enter stack number (1-3): 2
Stack 2 elements: 10
--- MENU ---
1. Push
2. Pop
3. Display
4. Exit
Enter choice: 3
Enter stack number (1-3): 3
Stack 3 elements: 10
--- MENU ---
1. Push
2. Pop
3. Display
4. Exit
Enter choice: 2
Enter stack number (1-3): 1
Popped 10 from Stack 1
--- MENU ---
1. Push
2. Pop
3. Display
4. Exit
Enter choice: 4
Exiting program...


## **Conclusion**

The multiple stack implementation using a single array demonstrates an efficient memory management technique that is particularly useful in scenarios where multiple stacks are needed but their individual sizes are predictable and fixed. This approach achieves optimal space utilization by dividing a contiguous memory block into equal segments, ensuring that each stack operates independently without interfering with others while sharing the same underlying storage structure. 