# String to Integer (atoi)

### Example 1:

```cpp
Input: "42"
Output: 42
```

### Example 2:

```cpp
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```

### Example 3:

```cpp
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

### Example 4:

```cpp
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```

### Example 5:

```cpp
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.
```

## Solution

This is the typical problem that might finely showcase both the candidate command of core programming concepts and their analytical skills, starting from the easiest cases and then improving their code step-by-step in order to manage all the edge-cases that the interviewer might have mentioned before starting to code (if properly questioned about them) or added later (maybe just to throw a few extra curveballs to a promising candidate).

In our approach we will try to follow this path, starting with the easiest case (the number is composed only of a small number of digits) and then moving on to tackle more challenging inputs.

### First step

Parsing the input one character at a time:

```cpp
class Solution {
public:
    int myAtoi(string str) {
        int res = 0;
        for (char c: str) {
            res = res * 10 + c - 48;
        }
        return res;
    }
};
```

Notice that we used the [range-based for loop](https://en.cppreference.com/w/cpp/language/range-for), available since C++11 - it is basically equivalent to:

```cpp
char c;
for (int i = 0; i < str.size(); i++) {
  c = str[i];
  // your looping magic here...
}
```

It just makes your code more concise and potentially readable, so we would rather recommend it when your code has no actual need for the index of the iterable your are currently looping.

Back to business, our solution should appear relatively straightforward: we initalise a variable that will hold our final result (`res`) and at each iteration we proceed to take the leftmost not previously scanned digit.

We then get its numerical value subtracting `48` (the ASCII code for the character `0`, see [here](https://en.wikipedia.org/wiki/ASCII#Printable_characters)) from it: C++ allows mathematical operations between `int`s and `char`s, sparing us the pain of creating some data structure or piece of logic to match them.

Finally, we add that value to `res * 10`, a convenient way to shift the previous `res` to the right and make room for our latest digit.

In a convenient example for `str` equal to `87651234`:

```cpp
// initialisation step
res = 0

// first iteration:
c == '8';
res = 0 * 10 + `8` - 48; // equals 8, since the product gives 0 and the value of c in ASCII code is 56

// second iteration:
c == '7';
res = 8 * 10 + `7` - 48; // equals 87, since the product gives 8 and the value of c in ASCII code is 55

// third iteration:
c == '6';
res = 87 * 10 + `6` - 48; // equals 876, since the product gives 6 and the value of c in ASCII code is 54

...

// last iteration:
c == '4';
res = 8765123 * 10 + `4` - 48; // equals 87651234, since the product gives 4 and the value of c in ASCII code is 52
```

From https://leetcode.com/problems/string-to-integer-atoi/
