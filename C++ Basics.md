# C++ Basics

## 1. Input and Output

### `cin` and `cout`

```cpp
cout << "Hello";  // Output

int age;
cin >> age;       // Input
```
 
### ⚠️ `cin >>` reads only one word

```cpp
string name;
cin >> name;
```

If the user enters:
```text
Sumit Kumar
```

`name` only becomes:
```text
Sumit
```

* **Reason:** `cin >>` stops at spaces.
* 👉 *Later, learn `getline()` for full lines.*

---

## 2. Variables and Data Types

### ⚠️ Integer division

If both numbers are integers:
```cpp
9 / 5
```

Result:
```text
1
```
*(The decimal part is discarded)*

#### Common mistake:
```cpp
float f = c * (9 / 5) + 32;
```
❌ `9 / 5` becomes `1`.

#### Correct:
```cpp
float f = c * 9.0 / 5.0 + 32;
```
or:
```cpp
float f = 9 * c / 5 + 32;
```

### ⚠️ Integer Truncation (Decimal Omission)
If you divide integers and the actual math result has a decimal part, assigning the result to an `int` variable simply **discards the decimal part**. It does **not** round off.

```cpp
int result = 15 / 10; // Mathematical result is 1.5
cout << result;       // Output: 1 (0.5 is omitted, no rounding to 2!)
```

---

## 3. Math & Exponentiation Traps

### ⚠️ `^` is NOT the power operator
In mathematics, \(10^{d-1}\) represents a power. However, in C++:
*   `^` means **bitwise XOR** (e.g., `10 ^ 2` performs a bitwise operation, not power).

#### Correct way to do powers:
Use the `pow()` function from the `<cmath>` library.

```cpp
#include <iostream>
#include <cmath> // Required for pow()
using namespace std;

int main() {
    // pow(base, exponent)
    double result = pow(10, 3); // 10 raised to the power 3 (1000)
    
    // ⚠️ Note: pow() returns a double (floating-point number)
    int intResult = pow(10, 2); // Can be cast to int, but watch out for precision loss
}
```

---

## 4. Array Boundary Traps

### ⚠️ Off-by-One / Out of Bounds Error in Loops
When accessing neighbor elements like `numbers[i + 1]` in a loop, you must stop the loop **one index before the last element** to prevent reading outside the array.

#### The Mistake:
```cpp
// Array of 6 elements: indices 0 to 5
int numbers[6] = {1, 2, 3, 4, 5, 6};

// ❌ Accesses numbers[6] on the final iteration (i = 5), which is out of bounds!
for (int i = 0; i <= 5; i++) {
    if (numbers[i] > numbers[i + 1]) { 
        sorted = false;
        break;
    }
}
```

#### The Fix:
Stop the loop at index `4` (i.e., `i < 5`), so the last comparison is `numbers[4] > numbers[5]`.
```cpp
// ✅ Correct: stops before reaching out of bounds
for (int i = 0; i < 5; i++) {
    if (numbers[i] > numbers[i + 1]) {
        sorted = false;
        break;
    }
}
```

👉 **Golden Rule:** When using `arr[i + 1]`, make sure your loop limit is adjusted to stop one element early.


### How to use getline(). 

If you do cin>>string, it will stop taking after there is a space in the input. 
so we use getline(); 
string name;
getline(cin, name);
cin.ignore() is needed sometimes. 
```
#include <iostream>
#include <string>
using namespace std;

int main() {

    int count = 0;

    string roll_number;
    string name;

    cout << "Enter your roll number: ";
    cin >> roll_number;

    cout << endl << "Enter your name: ";
    cin.ignore();
    getline(cin, name);

    cout << endl << roll_number << " " << name;

    return 0;
}
```