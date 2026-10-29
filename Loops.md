# C++ Conditions and Loops

---

## Part 1: Conditions

### 1. `if`, `else if`, and `else`
Used to execute different blocks of code based on different conditions.

#### Syntax
```cpp
if (condition1) {
    // Block of code if condition1 is true 
} else if (condition2) {
    // Block of code if condition1 is false and condition2 is true
} else {
    // Block of code if all conditions are false
}
```

#### Example
```cpp
int time = 22;
if (time < 10) {
    cout << "Good morning.";
} else if (time < 20) {
    cout << "Good day.";
} else {
    cout << "Good evening."; // This will run
}
```

---

### 2. `switch` Statement
Use `switch` to select one of many code blocks to be executed. It is often cleaner than writing multiple `else if` statements.

#### Syntax
```cpp
switch (expression) {
    case x:
        // code block
        break;
    case y:
        // code block
        break;
    default:
        // default code block if no match is found
}
```
*   `break`: Exits the switch block immediately. If omitted, execution "falls through" to the next case.
*   `default`: Runs if no case matches.

#### Example
```cpp
int day = 4;
switch (day) {
    case 6:
        cout << "Today is Saturday";
        break;
    case 7:
        cout << "Today is Sunday";
        break;
    default:
        cout << "Looking forward to the Weekend"; // This will run
}
```

---

## Part 2: Loops

Loops are used to execute a block of code repeatedly as long as a specified condition is met.

### 1. `while` Loop
Loops through a block of code as long as a specified condition is `true`.
*Note: The condition is checked **before** entering the loop.*

#### Syntax
```cpp
while (condition) {
    // code block to be executed
}
```

#### Example
```cpp
int i = 0;
while (i < 5) {
    cout << i << "\n";
    i++;
}
```

---

### 2. `do-while` Loop
It executes the code block **once** before checking the condition, and then repeats the loop as long as the condition is `true`.
*Note: Guaranteed to execute **at least once**.*

#### Syntax
```cpp
do {
    // code block to be executed
} while (condition);
```

#### Example
```cpp
int i = 0;
do {
    cout << i << "\n";
    i++;
} while (i < 5);
```

---

### 3. `for` Loop
Use when you know exactly how many times you want to loop through a block of code.

#### Syntax
```cpp
for (initialization; condition; increment/decrement) {
    // code block to be executed
}
```

#### Example
```cpp
for (int i = 0; i < 5; i++) {
    cout << i << "\n";
}
```

---

### 4. Range-Based `for` Loop (C++11)
Used exclusively to loop through elements in an array or a collection.

#### Syntax
```cpp
for (type variableName : arrayName) {
    // code block to be executed
}
```

#### Example
```cpp
int numbers[5] = {10, 20, 30, 40, 50};

for (int x : numbers) {
    cout << x << "\n";
}
```

---

### 5. Loop Control Statements (`break` & `continue`)

Used to alter the normal execution flow of loops.

---

### Quick Comparison

| Statement | What it does | Where execution goes | Can be used in |
| :--- | :--- | :--- | :--- |
| **`break;`** | **Terminates** the entire loop immediately 🚪 | Directly to the first line **after** the loop | Loops (`for`, `while`, `do-while`) & `switch` |
| **`continue;`** | **Skips** only the current iteration ⏭️ | Directly to the **next loop iteration** | Loops **only** (`for`, `while`, `do-while`) |

---

### 1. The `break` Statement

Exits and stops the loop completely, regardless of the loop condition.

#### Example: Early Exit When Found
```cpp
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        break; // Stop the loop completely when i reaches 3
    }
    cout << i << " ";
}
cout << "Done!";
```

**Output:**
```text
1 2 Done!
```

#### ⚠️ Trap 1: Nested Loops
`break` only exits the **innermost loop** it is currently inside — not outer loops!

```cpp
for (int i = 1; i <= 2; i++) {
    for (int j = 1; j <= 3; j++) {
        if (j == 2) {
            break; // Exits ONLY the inner 'j' loop!
        }
        cout << j << " ";
    }
    cout << "\n";
}
```

**Output:**
```text
1 
1 
```

---

### 2. The `continue` Statement

Stops the remaining code in the **current turn** and immediately moves to the next iteration.

#### Example: Skip Specific Values
```cpp
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue; // Skips printing 3, jumps to i++
    }
    cout << i << " ";
}
```

**Output:**
```text
1 2 4 5 
```

#### ⚠️ Trap 2: Infinite Loop in `while` Loops!
* In a `for` loop, `continue` jumps to the **increment step** (`i++`).
* In a `while` loop, if your increment is placed after `continue`, the increment is skipped, creating an **infinite loop**!

❌ **Buggy Code (Infinite Loop):**
```cpp
int i = 1;
while (i <= 5) {
    if (i == 3) {
        continue; // ❌ Skips i++, so i stays 3 forever!
    }
    cout << i << " ";
    i++;
}
```

✅ **Fixed Code:**
```cpp
int i = 1;
while (i <= 5) {
    if (i == 3) {
        i++;      // ✅ Increment before skipping!
        continue;
    }
    cout << i << " ";
    i++;
}
```

---

### Summary Checklist ⭐
* Use **`break`** when you found what you need and want to stop searching immediately.
* Use **`continue`** when you want to filter out unwanted items (e.g., skip even numbers or negative values).
* In nested loops, remember that `break` and `continue` only affect the **loop they are directly inside**.

