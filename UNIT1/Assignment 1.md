# Assignment 1  

## Title  
**Implementation of Basic String Operations (Length, Copy, Reverse, Concatenation) using Character Arrays in C++ without String Library Functions**  


---

## Theory  
In C++, a string is a sequence of characters terminated by the null character `'\0'`. When we use **character arrays** instead of the `string` class, all operations (length, copy, reverse, concatenation) must be performed manually.  

### Operations Implemented  
1. **String Length** – Traverse until `'\0'` and count characters.  
2. **String Copy** – Copy characters one by one into a new array.  
3. **String Reverse** – Swap characters from both ends until the middle.  
4. **String Concatenation** – Append the second string to the end of the first.  

**Dynamic Memory Allocation** (`new` operator) allows runtime allocation of memory so that strings of varying lengths can be handled flexibly.  

---

## Code  
```cpp
#include <iostream>
using namespace std;

int stringLength(char str[])
{
    int i = 0;
    while (str[i] != '\0') 
    {
        i++;
    }
    return i;
}

void stringCopy(char dest[], char src[]) 
{
    int i = 0;
    while (src[i] != '\0') 
    {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0'; 
}

void stringReverse(char str[]) 
{
    int len = stringLength(str);
    for (int i = 0; i < len / 2; i++) 
    {
        char temp = str[i];
        str[i] = str[len - i - 1];
        str[len - i - 1] = temp;
    }
}

void stringConcatenate(char result[], char str1[], char str2[]) 
{
    int i = 0, j = 0;

    while (str1[i] != '\0') 
    {
        result[i] = str1[i];
        i++;
    }

    
    while (str2[j] != '\0') 
    {
        result[i] = str2[j];
        i++;
        j++;
    }

    result[i] = '\0'; 
}

int main() 
{
    char str1[100], str2[100], result[200], copied[100];

    cout << "Enter first string: ";
    cin.getline(str1, 100);

    cout << "Enter second string: ";
    cin.getline(str2, 100);

   
    int len1 = stringLength(str1);
    int len2 = stringLength(str2);
    cout << "\nLength of first string: " << len1 << endl;
    cout << "Length of second string: " << len2 << endl;

   
    stringCopy(copied, str1);
    cout << "Copied first string: " << copied << endl;

    
    stringReverse(str1);
    cout << "Reversed first string: " << str1 << endl;

    stringConcatenate(result, copied, str2);
    cout << "Concatenated string : " << result << endl;

    return 0;
}

```
---
## Output
PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS1.exe'
Enter first string: Kanchan
Enter second string: Kapil

Length of first string: 7
Length of second string: 5
Copied first string: Kanchan
Reversed first string: nahcnaK
Concatenated string : KanchanKapil

---
## Conclusion  
In this program, we implemented fundamental string operations such as length calculation, copy, reverse, and concatenation using character arrays in C++ without relying on built-in string functions.