# C++ Functions

A **function** is a reusable block of code designed to perform a specific task.

---

## 1. Function Syntax & Anatomy

```cpp
returnType functionName(parameter1, parameter2, ...) {
    // Code body
    return value; // (Omit if returnType is void)
}
```

* **Return Type:** The type of value the function sends back (`int`, `double`, `string`, `bool`, or `void` if nothing is returned).
* **Function Name:** An identifier following naming conventions (e.g., camelCase or snake_case).
* **Parameters:** Input values accepted by the function (optional).
* **Body:** The instructions executed when the function is called.

### Basic Example:
```cpp
#include <iostream>
using namespace std;

// Function definition
int add(int a, int b) {
    return a + b;
}

int main() {
    int sum = add(5, 3); // Function call (Arguments: 5, 3)
    cout << sum;         // Output: 8
    return 0;
}
```

---

## 2. Function Prototype (Declaration vs. Definition)

C++ compiles code from top to bottom. If you call a function **before** its definition, the compiler throws an error.

To solve this, use a **Function Prototype** (declaration) at the top, and define the function later:

```cpp
#include <iostream>
using namespace std;

// 1. Function Prototype (Declaration)
void greet(); 

int main() {
    greet(); // ✅ Works because compiler knows the function exists
    return 0;
}

// 2. Function Definition
void greet() {
    cout << "Hello, World!\n";
}
```

---

## 3. Parameters vs. Arguments

* **Parameter:** The variable listed in the function definition header.
* **Argument:** The actual value passed into the function when calling it.

```cpp
void printAge(int age) { // 'age' is a PARAMETER
    cout << age;
}

int main() {
    printAge(25);        // '25' is an ARGUMENT
}
```

---

## 4. Parameter Passing Methods (CRITICAL ⭐)

There are three primary ways to pass data to functions in C++:

### A. Pass by Value (Default)
* A **copy** of the argument is passed into the function.
* Changes made inside the function **do NOT affect** the original variable.

```cpp
void modifyValue(int x) {
    x = 100; // Only changes the local copy!
}

int main() {
    int num = 10;
    modifyValue(num);
    cout << num; // Output: 10 (unchanged)
}
```

---

### B. Pass by Reference (`&`)
* Passes an **alias/reference** to the original variable directly (no copy).
* Changes made inside the function **DO modify** the original variable.

```cpp
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

int main() {
    int x = 5, y = 10;
    swap(x, y);
    cout << x << " " << y; // Output: 10 5 (successfully swapped!)
}
```

---

### C. Pass by Const Reference (`const Type&`) ⭐ *Best Practice*
* Passes by reference (avoiding the performance overhead of copying large objects like `string`, `vector`, or `struct`).
* The `const` keyword prevents the function from modifying the original variable.

```cpp
// Efficient: No copying of string data, yet completely safe from modification
void printMessage(const string& msg) {
    cout << msg << "\n";
    // msg = "New"; // ❌ COMPILE ERROR: msg is read-only
}
```

---

### D. Pass by Pointer (`*`)
* Passes the **memory address** of the variable.
* Requires the caller to use `&` and the function to dereference using `*`.

```cpp
void reset(int* ptr) {
    if (ptr != nullptr) {
        *ptr = 0; // Modifies original value
    }
}

int main() {
    int val = 42;
    reset(&val);
    cout << val; // Output: 0
}
```

### Parameter Passing Comparison Table

| Method | Syntax | Copies Data? | Can Modify Original? | Best Used For |
| :--- | :--- | :--- | :--- | :--- |
| **Pass by Value** | `void f(int x)` | Yes | No | Small/primitive types (`int`, `char`, `bool`) |
| **Pass by Reference** | `void f(int& x)` | No | **Yes** | When you need to modify or return multiple values |
| **Pass by Const Ref** | `void f(const string& s)` | No | No | Large objects (`string`, `vector`, `struct`) |
| **Pass by Pointer** | `void f(int* ptr)` | No (copies 8B address) | **Yes** | Optional inputs (`nullptr`) or C-APIs |

---

## 5. Default Arguments

You can provide default values for parameters. If the caller omits an argument, the default is used.

> ⚠️ **Rule:** Default arguments must be placed at the **end** (rightmost parameters) of the parameter list.

```cpp
void displayInfo(string name, int age = 18) {
    cout << name << " is " << age << " years old.\n";
}

int main() {
    displayInfo("Alice");     // Output: Alice is 18 years old. (uses default)
    displayInfo("Bob", 25);   // Output: Bob is 25 years old.
}
```

*Note:* If using function prototypes, specify the default argument in the **prototype only**, not in both prototype and definition!

---

## 6. Function Overloading

**Function Overloading** allows multiple functions to share the **same name**, as long as their parameter lists are different (different number of parameters, different types, or different order).

> ⚠️ **Crucial Rule:** You cannot overload functions by changing the **return type alone**!

```cpp
int multiply(int a, int b) {
    return a * b;
}

double multiply(double a, double b) {
    return a * b;
}

int multiply(int a, int b, int c) {
    return a * b * c;
}

int main() {
    cout << multiply(2, 3) << "\n";       // Calls int version (6)
    cout << multiply(2.5, 4.0) << "\n";   // Calls double version (10.0)
    cout << multiply(2, 3, 4) << "\n";    // Calls 3-argument version (24)
}
```

---

## 7. Recursion (Recursive Functions)

A function that calls **itself** to break down a problem into simpler sub-problems.

Every recursive function must have two components:
1. **Base Case:** Condition that stops recursion (prevents infinite loops).
2. **Recursive Case:** Calls the function with modified arguments moving toward the base case.

### Example: Factorial of $n$ ($n! = n \times (n-1)!$)
```cpp
int factorial(int n) {
    // 1. Base Case
    if (n <= 1) {
        return 1;
    }
    // 2. Recursive Case
    return n * factorial(n - 1);
}

int main() {
    cout << factorial(5); // Output: 120 (5 * 4 * 3 * 2 * 1)
}
```

> ⚠️ **Stack Overflow Danger:** If the base case is missing or unreachable, the function keeps calling itself until stack memory is exhausted, crashing the program!

---

## 8. Inline Functions (`inline`)

Calling a function carries slight performance overhead (pushing arguments to the stack, jumping memory addresses, returning).

For very short (1-2 line) functions, the `inline` keyword suggests the compiler replace the function call directly with the function code inline.

```cpp
inline int square(int x) {
    return x * x;
}

int main() {
    int res = square(5); 
    // Compiler may transform this directly into: int res = 5 * 5;
}
```

---

## 9. Variable Scope & `static` Local Variables

* **Local Variables:** Declared inside a function; created on entry and destroyed when the function exits.
* **Global Variables:** Declared outside all functions; accessible anywhere (generally discouraged).
* **`static` Local Variables:** Preserves its value between function calls! Initialized only once.

```cpp
void counter() {
    static int count = 0; // Initialized once; retains value across calls
    count++;
    cout << "Count: " << count << "\n";
}

int main() {
    counter(); // Count: 1
    counter(); // Count: 2
    counter(); // Count: 3
}
```

---

## 10. Lambda Functions (Modern C++11)

A **lambda** is an anonymous, unnamed function you can define inline right where you need it.

### Syntax
```text
[capture](parameters) -> returnType { body };
```

```cpp
#include <iostream>
using namespace std;

int main() {
    // Simple lambda
    auto greet = []() {
        cout << "Hello from Lambda!\n";
    };
    greet();

    // Lambda with parameters and return value
    auto add = [](int a, int b) -> int {
        return a + b;
    };
    cout << add(4, 6); // Output: 10
}
```

---

## 11. ⚠️ Critical Pitfalls & Golden Rules

### 1. The Dangling Pointer / Reference Trap 🚨
**Never return a reference or pointer to a local variable!**  
Local variables are destroyed when the function finishes. The returned pointer/reference will point to deallocated garbage memory.

```cpp
// ❌ DANGEROUS: Undefined behavior!
int& getLocalRef() {
    int x = 10;
    return x; // Warning: reference to local variable 'x' returned
}
```

### 2. Missing Return Statement
Non-void functions **must** return a value on all possible execution paths. Failing to return leads to undefined behavior.

```cpp
// ❌ Buggy: what if n is negative or zero?
int check(int n) {
    if (n > 0) return 1;
    // Missing return here!
}
```
