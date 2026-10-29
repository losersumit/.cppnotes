# C++ Operators

## 1. Arithmetic Operators
Used to perform basic mathematical operations.

| Operator | Description | Example (`int a = 10, b = 3;`) |
| :---: | --- | --- |
| `+` | Addition | `a + b` ➔ `13` |
| `-` | Subtraction | `a - b` ➔ `7` |
| `*` | Multiplication | `a * b` ➔ `30` |
| `/` | Division | `a / b` ➔ `3` *(decimal part discarded)* |
| `%` | Modulo (Remainder) | `a % b` ➔ `1` | 

---

## 2. Assignment Operators
Used to assign values to variables.

* `=` : Simple assignment (`a = 5;`)
* `+=` : Add and assign (`a += 3;` is the same as `a = a + 3;`)
* `-=` : Subtract and assign (`a -= 2;`)
* `*=` : Multiply and assign (`a *= 4;`)
* `/=` : Divide and assign (`a /= 2;`)
* `%=` : Modulo and assign (`a %= 3;`)

---

## 3. Comparison / Relational Operators
Used to compare two values. They return `true` (1) or `false` (0).

| Operator | Description | Example (`a = 5, b = 10;`) |
| :---: | --- | --- |
| `==` | Equal to | `a == b` ➔ `false` |
| `!=` | Not equal to | `a != b` ➔ `true` |
| `>` | Greater than | `a > b` ➔ `false` |
| `<` | Less than | `a < b` ➔ `true` |
| `>=` | Greater than or equal to | `a >= 5` ➔ `true` |
| `<=` | Less than or equal to | `b <= 10` ➔ `true` |

---

## 4. Logical Operators
Used to combine multiple conditions.

### `&&` (Logical AND)
Returns `true` only if **both** conditions are true.
```cpp
(5 > 3 && 8 > 5)  // true
(5 > 3 && 2 > 5)  // false
```

### `||` (Logical OR)
Returns `true` if **at least one** condition is true.
```cpp
(5 > 10 || 3 > 2) // true
(5 > 10 || 1 > 2) // false
```

### `!` (Logical NOT)
Reverses the result (turns `true` to `false` and vice versa).
```cpp
!(5 > 3)          // false
```

---

## 5. Increment and Decrement Operators
Used to increase or decrease a variable's value by 1.

* `++x` (Prefix) / `x++` (Postfix) : Increases value by 1
* `--x` (Prefix) / `x--` (Postfix) : Decreases value by 1

### Prefix vs Postfix Example:
```cpp
int a = 5;
int b = ++a; // a becomes 6, b becomes 6 (increments first, then assigns)

int x = 5;
int y = x++; // x becomes 6, y becomes 5 (assigns first, then increments)
```

---

## 6. Ternary Operator (`? :`)
A short way to write an `if-else` statement.

```cpp
// Syntax: condition ? expression_if_true : expression_if_false;

int time = 20;
string result = (time < 18) ? "Good day." : "Good evening."; 
// result becomes "Good evening."
```
