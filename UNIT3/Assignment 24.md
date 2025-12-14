# **Assignment 24**

## **Title**

You are given a string containing only parentheses characters: '(', ')', '{', '}', '[', and ']'. Your task is to check whether the parentheses are balanced or not.

A string is considered balanced if:
1. Every opening bracket has a corresponding closing bracket of the same type
2. Brackets are closed in the correct order

## **Theory**

The balanced parentheses problem is a classic application of the stack data structure. The key insight is that brackets must be closed in the reverse order of how they were opened, which perfectly matches the Last-In-First-Out (LIFO) behavior of stacks.

**Key Concepts:**

1. **Stack Usage**:
   - Push opening brackets '(', '{', '[' onto the stack
   - When a closing bracket is encountered, check if it matches the top of stack
   - Pop the matching opening bracket from stack
   - If no match found or stack is empty, brackets are unbalanced

2. **Matching Rules**:
   - ')' matches with '('
   - '}' matches with '{'
   - ']' matches with '['

3. **Balance Conditions**:
   - At the end, stack must be empty (all opening brackets matched)
   - Every closing bracket must have a corresponding opening bracket
   - Brackets must be in correct nested order

**Algorithm Steps:**

1. **Initialize** an empty stack
2. **Traverse** the string character by character
3. **For each character**:
   - If it's an opening bracket: Push it onto the stack
   - If it's a closing bracket:
     - Check if stack is empty (no matching opening bracket) → Unbalanced
     - Check if top of stack matches current closing bracket
     - If matches: Pop from stack
     - If doesn't match → Unbalanced
4. **After traversal**: Check if stack is empty
   - Empty stack → Balanced
   - Non-empty stack → Unbalanced (unclosed opening brackets)

**Examples:**

- **Balanced**: "()", "[]", "{}", "([])", "{[()]}", "[({})]"
- **Unbalanced**: "(", ")", ")(", "([)]", "{[}", "(()"
## **Step-by-Step Example: {[()]}**

Let's trace through the string **{[()]}**:

**Initial State:**
- Stack: Empty
- String: {[()]}

**Step 1: Read '{'**
- Opening bracket: Push to stack
- Stack: {
- Status: Continue

**Step 2: Read '['**
- Opening bracket: Push to stack
- Stack: {, [
- Status: Continue

**Step 3: Read '('**
- Opening bracket: Push to stack
- Stack: {, [, (
- Status: Continue

**Step 4: Read ')'**
- Closing bracket: Check stack top
- Stack top: '(' matches with ')'
- Pop from stack
- Stack: {, [
- Status: Continue

**Step 5: Read ']'**
- Closing bracket: Check stack top
- Stack top: '[' matches with ']'
- Pop from stack
- Stack: {
- Status: Continue

**Step 6: Read '}'**
- Closing bracket: Check stack top
- Stack top: '{' matches with '}'
- Pop from stack
- Stack: Empty
- Status: Continue

**End of String:**
- Stack is empty
- Result: **BALANCED ✓**
---
## **Code**

```cpp
#include <iostream>
#include <stack>
using namespace std;

bool isBalanced(string s)
{
    stack<char> st;

    for (char ch : s)
    {

        if (ch == '(' || ch == '{' || ch == '[')
        {
            st.push(ch);
        }

        
        else if (ch == ')' || ch == '}' || ch == ']')
        {

            if (st.empty())
                return false; 

            char top = st.top();
            st.pop();

           
            if ((ch == ')' && top != '(') ||
                (ch == '}' && top != '{') ||
                (ch == ']' && top != '['))
            {
                return false;
            }
        }
    }

    return st.empty();
}

int main()
{
    string s;
    cout << "Enter string: ";
    cin >> s;

    if (isBalanced(s))
        cout << "Balanced";
    else
        cout << "Not Balanced";

    return 0;
}

```
## **Output**

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS24.exe'
Enter string: [({})]
Balanced

## **Conclusion**

The balanced parentheses checker successfully demonstrates the power and elegance of stack-based solutions for matching problems. This implementation provides an efficient algorithm with linear time complexity O(n) where n is the length of the input string, as each character is processed exactly once with constant-time stack operations. The space complexity is also O(n) in the worst case when all characters are opening brackets that need to be stored in the stack.