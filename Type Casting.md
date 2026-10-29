# C++ Type Casting

Type casting is the process of converting a value from one data type to another.

---

## 1. Why Do We Need Type Casting?
In C++, the data types involved in a calculation determine how the calculation is performed. 
* **`int` / `int` ➔ `int`** (e.g., `5 / 2 = 2`)
* **`float` / `int` ➔ `float`** (e.g., `5.0 / 2 = 2.5`)

To get decimal results from integer variables, we must temporarily convert one of the variables.

---

## 2. The Assignment Trap
⚠️ **The calculation happens BEFORE the value is stored.**
```cpp
int n = 5;
float x = n / 2; // C++ calculates (5 / 2) which is 2, then stores it as 2.0!
```
To fix this, cast one of the numbers before the division occurs:
```cpp
float x = (float)n / 2; // Calculates (5.0 / 2) which is 2.5
```

---

## 3. How to Cast Types in C++

### A. Old-School (C-Style) Casting
The traditional way to cast is by placing the target data type in parentheses before the variable.
```cpp
(float)variable  // Converts to float
(int)variable    // Converts to int
(char)variable   // Converts to char
```

### B. Modern (C++) Casting: `static_cast`
In modern C++, it is preferred to use `static_cast`. It is safer and easier to spot in code.
```cpp
static_cast<float>(variable)
static_cast<int>(variable)
```

---

## 4. Does Casting Change the Original Variable?
**No.** Casting only creates a temporary converted value for that specific calculation.
```cpp
int n = 5;
float x = (float)n;

// n is still 5 (int)
// x is now 5.0 (float)
```

---

## 5. Implicit vs. Explicit Conversion

* **Implicit (Automatic) Conversion**: Done automatically by the compiler.
  ```cpp
  int n = 5;
  float x = n; // C++ automatically converts 5 to 5.0
  ```
* **Explicit (Manual) Conversion**: Handled by you using type casting.
  ```cpp
  float x = 5.9;
  int n = (int)x; // Manually casting float to int
  ```

---

## 6. Common Conversions

### `float` ➔ `int`
*The decimal part is discarded entirely (truncated), not rounded!*
```cpp
float x = 5.9;
int n = static_cast<int>(x); // n becomes 5
```

### `char` ➔ `int`
*Converts a character to its ASCII value.*
```cpp
char c = 'A';
int ascii = (int)c; // ascii becomes 65
```

### `int` ➔ `char`
*Converts an ASCII number back to its character.*
```cpp
int code = 65;
char c = (char)code; // c becomes 'A'
```

---

## Golden Rule ⭐
> **Only one operand in a division needs to be cast to a decimal type to get a decimal result.**
> ```cpp
> (float)a / b  // Decimal division ✅
> a / (float)b  // Decimal division ✅
> ```
