# **Assignment 22**

## **Title**

Convert given infix expression Eg. a-b*c-d/e+f into postfix form using stack and show the operations step by step.

## **Theory**

Infix notation is the standard mathematical notation where operators are placed between operands (e.g., A + B). Postfix notation (also known as Reverse Polish Notation) places operators after their operands (e.g., A B +). Converting infix to postfix eliminates the need for parentheses and makes expression evaluation straightforward using a stack.

**Key Concepts:**

1. **Operator Precedence**:
   - Multiplication (*) and Division (/) have higher precedence than Addition (+) and Subtraction (-)
   - Operators with higher precedence are evaluated first
   - Precedence order: `* / > + -`

2. **Operator Associativity**:
   - All arithmetic operators are left-associative
   - When operators have the same precedence, evaluate from left to right

3. **Stack Usage**:
   - Stack stores operators temporarily during conversion
   - Operands are directly added to the output
   - Operators are pushed/popped based on precedence rules

**Algorithm Steps:**

1. **Initialize**: Create an empty stack for operators and an empty string for output
2. **Scan** the infix expression from left to right
3. **For each character**:
   - If it's an **operand** (variable/number): Add it directly to the output
   - If it's an **operator**:
     - Pop operators from stack to output while stack top has greater or equal precedence
     - Push the current operator onto the stack
   - If it's an **opening parenthesis** '(': Push it onto the stack
   - If it's a **closing parenthesis** ')': Pop operators to output until '(' is found
4. **After scanning**: Pop all remaining operators from stack to output
5. **Result**: The output string is the postfix expression

**Precedence Rules:**
- `*` and `/` have precedence value 2
- `+` and `-` have precedence value 1
- When current operator has lower or equal precedence than stack top, pop the stack

## **Step-by-Step Example: a-b*c-d/e+f**

Let's trace through the conversion of the expression **a-b*c-d/e+f**:

**Initial State:**
- Stack: Empty
- Postfix: ""

**Step 1: Read 'a'**
- Operand: Add to postfix
- Stack: Empty
- Postfix: a

**Step 2: Read '-'**
- Operator: Stack empty, push '-'
- Stack: -
- Postfix: a

**Step 3: Read 'b'**
- Operand: Add to postfix
- Stack: -
- Postfix: ab

**Step 4: Read '*'**
- Operator: '*' has higher precedence than '-', push '*'
- Stack: -, *
- Postfix: ab

**Step 5: Read 'c'**
- Operand: Add to postfix
- Stack: -, *
- Postfix: abc

**Step 6: Read '-'**
- Operator: Pop '*' (higher precedence), pop '-' (equal precedence)
- Push current '-'
- Stack: -
- Postfix: abc*-

**Step 7: Read 'd'**
- Operand: Add to postfix
- Stack: -
- Postfix: abc*-d

**Step 8: Read '/'**
- Operator: '/' has higher precedence than '-', push '/'
- Stack: -, /
- Postfix: abc*-d

**Step 9: Read 'e'**
- Operand: Add to postfix
- Stack: -, /
- Postfix: abc*-de

**Step 10: Read '+'**
- Operator: Pop '/' (higher precedence), pop '-' (equal precedence)
- Push current '+'
- Stack: +
- Postfix: abc*-de/-

**Step 11: Read 'f'**
- Operand: Add to postfix
- Stack: +
- Postfix: abc*-de/-f

**Step 12: End of Expression**
- Pop all remaining operators: '+'
- Stack: Empty
- Postfix: abc*-de/-f+

**Final Answer: a-b*c-d/e+f → abc*-de/-f+**

## **Code**

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <iomanip> 
#include <vector>
#include <algorithm>
using namespace std;

int prec(char op)
{
    if (op == '+' || op == '-')
        return 1;
    if (op == '*' || op == '/')
        return 2;
    return 0;
}


string stackToString(stack<char> s)
{
    vector<char> v;
    while (!s.empty())
    {
        v.push_back(s.top());
        s.pop();
    }
    reverse(v.begin(), v.end());
    string out;
    for (char c : v)
        out += c, out += ' ';
    if (!out.empty())
        out.pop_back();
    return out;
}


void showStep(int step, char token, const string &action, stack<char> st, const string &out)
{
    cout << setw(3) << step << " | ";
    cout << setw(6) << token << " | ";
    cout << setw(30) << left << action.substr(0, 30) << " | ";
    cout << setw(20) << left << stackToString(st) << " | ";
    cout << out << '\n';
}


string infixToPostfixStepByStep(const string &infix)
{
    stack<char> st;
    string postfix;
    int step = 0;

    cout << "Step | Token  | Action                        | Stack (bottom->top)    | Output\n";
    cout << "-----+--------+-------------------------------+------------------------+----------------\n";

    for (char ch : infix)
    {
        if (ch == ' ')
            continue;

        if (isalnum(ch))
        {
            postfix.push_back(ch);
            string action = string("Operand -> append '") + ch + "'";
            showStep(++step, ch, action, st, postfix);
        }
        else if (ch == '(')
        {
            st.push(ch);
            showStep(++step, ch, "Push '(' onto stack", st, postfix);
        }
        else if (ch == ')')
        {
            string action = "Pop until '('";
            while (!st.empty() && st.top() != '(')
            {
                postfix.push_back(st.top());
                st.pop();
            }
            if (!st.empty() && st.top() == '(')
                st.pop();
            showStep(++step, ch, action, st, postfix);
        }
        else
        {
            while (!st.empty() && prec(st.top()) >= prec(ch))
            {
                postfix.push_back(st.top());
                st.pop();
            }
            st.push(ch);
            string action = string("Push operator '") + ch + "'";
            showStep(++step, ch, action, st, postfix);
        }
    }

    while (!st.empty())
    {
        char top = st.top();
        st.pop();
        if (top == '(' || top == ')')
            continue;
        postfix.push_back(top);
        string action = string("Pop remaining operator '") + top + "'";
        showStep(++step, top, action, st, postfix);
    }

    return postfix;
}

int main()
{
    string infix;
    cout << "Enter infix expression (e.g. a-b*c-d/e+f): ";
    getline(cin, infix);

    string postfix = infixToPostfixStepByStep(infix);
    cout << "-----\nFinal Postfix: " << postfix << '\n';
    return 0;
}

```
## **Output**

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS22.exe'
Enter infix expression (e.g. a-b*c-d/e+f): a-b*c-d/e+f
Step | Token  | Action                        | Stack (bottom->top)    | Output
-----+--------+-------------------------------+------------------------+----------------
  1 |      a | Operand -> append 'a'          |                      | a
2   | -      | Push operator '-'              | -                    | a
3   | b      | Operand -> append 'b'          | -                    | ab
4   | *      | Push operator '*'              | - *                  | ab
5   | c      | Operand -> append 'c'          | - *                  | abc
6   | -      | Push operator '-'              | -                    | abc*-
7   | d      | Operand -> append 'd'          | -                    | abc*-d
8   | /      | Push operator '/'              | - /                  | abc*-d
9   | e      | Operand -> append 'e'          | - /                  | abc*-de
10  | +      | Push operator '+'              | +                    | abc*-de/-
11  | f      | Operand -> append 'f'          | +                    | abc*-de/-f
12  | +      | Pop remaining operator '+'     |                      | abc*-de/-f+

Final Postfix: abc*-de/-f+

## **Conclusion**

The infix to postfix conversion algorithm successfully demonstrates the practical application of stack data structure in compiler design and expression evaluation. This implementation provides a systematic approach to eliminate parentheses and operator precedence ambiguity from mathematical expressions, making them easier to evaluate programmatically. 