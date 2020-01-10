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

### First step - parsing just digits

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

Notice that this approach allows us to keep a **O(1)** space complexity, without the need to store intermediate values in vectors or arrays, delivering good performance just incrementing our result variable at each step.

Now, with the main part of our solution mostly done, we can focus on getting the next crucial step - handling the sign.

### Second Step - the Sign

Now things start to get a bit more complicated and we need to take into account the sign; one of the best approaches is probably to store the sign in another variable; you might use confidently a boolean for now in order to store this value. Later on we might change our pattern in order to more elegantly handly a few more edge cases.

So, updating our previous code, we add some conditional flow inside our loops and a final condition at the very last line - the `return` statement:

```cpp
class Solution {
public:
    int myAtoi(string str) {
        int res = 0;
        bool isPositive = true; // defaulting to positive values, when no sign is provided
        for (char c: str) {
            if (c > 47 && c < 58) { // this is the range for numerical characters, as shown in the above reference
                res = res * 10 + c - 48;
            }
            else if (c == 43 || c == 45) { // this checks if the character is either a plus (+) or a minus (-)
                isPositive = c == '+';
            }
        }
        return isPositive ? res : -res;
    }
};
```

Ok, almost done and we managed to keep our logic rather clean and performing. Notice that our solution also works for some cases that are not purely numerical, as white spaces or other characters are just ignored.

The last step, regrettably, will add some more complication that we deserves a more detailed discussion.

From https://leetcode.com/problems/string-to-integer-atoi/
