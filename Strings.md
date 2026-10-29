# C++ String Library (`std::string`)

To use strings in C++, you must include the `<string>` header:
```cpp
#include <iostream>
#include <string>
using namespace std;
```

---

## 1. String Input & Output

### `cin >>` vs `getline()`

| Method | Behavior | Use Case |
| :--- | :--- | :--- |
| `cin >> str;` | Stops reading at **whitespace** (space, tab, newline). | Reading single words. |
| `getline(cin, str);` | Reads the **entire line** until Enter (newline). | Reading full names, sentences with spaces. |

```cpp
string word;
cin >> word; // If input is "John Doe", word only gets "John"

string fullSentence;
getline(cin, fullSentence); // Reads "John Doe" completely
```

---

## 2. Capacity & Size Functions

| Function | Simple Meaning | Example (`string s = "Hello";`) | Result |
| :--- | :--- | :--- | :--- |
| `s.length()` | Number of characters in string | `s.length()` | `5` |
| `s.size()` | Identical to `.length()` (preferred in modern C++) | `s.size()` | `5` |
| `s.empty()` | Returns `true` if string is empty (`""`), else `false` | `s.empty()` | `false` |
| `s.clear()` | Deletes all characters, leaving it empty | `s.clear();` | `s` becomes `""` |
| `s.capacity()` | Total allocated memory before resizing is needed | `s.capacity()` | $\ge 5$ |
| `s.resize(n)` | Resizes string to length `n` (truncates or pads) | `s.resize(3);` | `s` becomes `"Hel"` |

---

## 3. Element Access Functions

How to read or edit individual characters inside a string:

### A. Index Operator `[]` vs `.at()`
* **`s[i]`**: Fast, but has **no boundary check**. Accessing out of bounds causes undefined behavior / crashes.
* **`s.at(i)`**: Safe. Throws an `out_of_range` error if index is invalid.

```cpp
string s = "Code";
cout << s[0];    // 'C'
cout << s.at(1); // 'o'

// s[10]    -> ❌ Undefined behavior / random memory read!
// s.at(10) -> ⚠️ Throws std::out_of_range exception
```

### B. First & Last Characters
* **`s.front()`**: Returns the first character (`s[0]`).
* **`s.back()`**: Returns the last character (`s[s.size() - 1]`).

### C. Converting to C-Style String (`const char*`)
* **`s.c_str()`**: Returns a null-terminated pointer `const char*`. Used when passing C++ strings into older C libraries.

---

## 4. Modifiers (Changing Strings)

### A. Adding Characters / Concatenation
* **`+` or `+=`**: Concatenates strings or characters.
  ```cpp
  string s = "Hello";
  s += " World"; // "Hello World"
  ```
* **`s.append(str)`**: Appends another string to the end.
  ```cpp
  s.append("!!!"); // "Hello World!!!"
  ```
* **`s.push_back(ch)`**: Appends a **single character** to the end.
  ```cpp
  s.push_back('?'); // Adds '?'
  ```

### B. Removing Characters
* **`s.pop_back()`** *(C++11)*: Removes the last character.
  ```cpp
  string s = "Hello!";
  s.pop_back(); // s becomes "Hello"
  ```
* **`s.erase(pos, count)`**: Erases `count` characters starting at index `pos`.
  ```cpp
  string s = "ABCDEF";
  s.erase(2, 3); // Erase 3 characters starting at index 2 ('C', 'D', 'E')
  cout << s;     // "ABF"
  ```

### C. Inserting & Replacing
* **`s.insert(pos, str)`**: Inserts `str` at index `pos`.
  ```cpp
  string s = "Hello World";
  s.insert(5, " Beautiful"); // "Hello Beautiful World"
  ```
* **`s.replace(pos, count, new_str)`**: Replaces `count` characters at `pos` with `new_str`.
  ```cpp
  string s = "I like apples";
  s.replace(7, 6, "oranges"); // "I like oranges"
  ```
* **`s.swap(other_s)`**: Swaps contents of two strings instantly without copying data.

---

## 5. Substrings & Searching

### A. `s.substr(pos, count)`
Extracts a portion of the string starting at index `pos` with a length of `count`.
> ⚠️ **Important:** Second argument is **LENGTH**, NOT the ending index!

```cpp
string s = "Programming";
string sub = s.substr(0, 7); // Start at index 0, take 7 characters
cout << sub; // "Program"

string tail = s.substr(7);   // From index 7 to the very end
cout << tail; // "ming"
```

### B. `s.find(str)`
Finds the **first occurrence** of `str`. Returns the **0-based index**.
* If not found, it returns the special constant **`string::npos`**.

```cpp
string text = "learn c++ programming";
size_t found = text.find("c++");

if (found != string::npos) {
    cout << "Found at index: " << found << "\n"; // Index 6
} else {
    cout << "Not found!\n";
}
```

### Other Search Functions:
* **`s.rfind(str)`**: Reverse find (searches backwards for the last occurrence).
* **`s.find_first_of(chars)`**: Finds the first character that matches *any* character in `chars`.
* **`s.find_first_not_of(chars)`**: Finds the first character that does *not* match any character in `chars`.

---

## 6. String & Number Conversions

Included in `<string>`:

### A. Number ➔ String
* **`to_string(num)`**: Converts `int`, `float`, `double`, `long long` to a `string`.
```cpp
int age = 25;
string s = "Age: " + to_string(age); // "Age: 25"
```

### B. String ➔ Number
* **`stoi(s)`**: String to `int`
* **`stod(s)`**: String to `double`
* **`stof(s)`**: String to `float`
* **`stoll(s)`**: String to `long long`

```cpp
string price = "199.99";
double d = stod(price); // 199.99 as a double
```

---

## 7. ⚠️ Common Mistakes & Traps

### ❌ Mistake 1: The `cin >>` then `getline()` Newline Trap
When you use `cin >>` followed by `getline()`, `cin >>` leaves the newline `\n` in the input buffer. `getline()` immediately reads that empty newline and skips your actual input!

```cpp
int age;
string name;

cin >> age;          // User types 20 and hits Enter
// The '\n' is still sitting in the buffer!
cin.ignore();        // ✅ FIX: Clears the leftover newline character
getline(cin, name);  // Now properly waits for user input!
```

---

### ❌ Mistake 2: Adding Two String Literals with `+`
In C++, `"Hello "` and `"World"` are `const char[]`, NOT `std::string`. You cannot add two raw string literals directly.

```cpp
// ❌ COMPILE ERROR: Cannot add two pointers!
string s = "Hello " + "World"; 

// ✅ Correct Ways:
string s = string("Hello ") + "World"; // At least one is a std::string
string s = "Hello "; s += "World";
```

---

### ❌ Mistake 3: Comparing `find()` with `-1`
`s.find()` returns an unsigned integer type (`size_t`). When an item is not found, it returns `string::npos`. Comparing with `-1` can cause unexpected compiler warnings or signed/unsigned mismatch bugs.

```cpp
// ❌ Bad Practice:
if (s.find("abc") == -1) 

// ✅ Correct:
if (s.find("abc") == string::npos) 
```

---

### ❌ Mistake 4: Single Quotes vs Double Quotes
* Single quotes `'a'` represent a single **`char`**.
* Double quotes `"a"` represent a **`string`** (`const char[2]` with `\0`).

```cpp
string s = "hello";
s += 'a';  // ✅ Appends character 'a'
s += "a";  // ✅ Appends string "a"

// s = 'hello'; // ❌ COMPILE ERROR: Multi-character literal
```

---

### ❌ Mistake 5: Passing `string` by Value to Functions
Passing `string` by value creates a duplicate copy of the entire string in memory every time the function is called.

```cpp
// ❌ Inefficient: Copies the entire string
void printString(string str);

// ✅ Best Practice: Passes by reference without copying, read-only
void printString(const string& str);
```

---

## Quick Reference Summary 📋

| Function | What it does |
| :--- | :--- |
| `size()` / `length()` | Returns number of characters |
| `empty()` | Checks if string is empty |
| `clear()` | Empties the string |
| `front()` / `back()` | Gets first / last character |
| `push_back(ch)` / `pop_back()` | Adds / removes character at the end |
| `append(str)` / `+=` | Adds string to end |
| `insert(pos, str)` | Inserts at specific index |
| `erase(pos, len)` | Erases `len` characters from `pos` |
| `substr(pos, len)` | Extracts substring of length `len` from `pos` |
| `find(str)` | Searches for substring (returns index or `string::npos`) |
| `to_string(val)` | Converts number to string |
| `stoi(s)` / `stod(s)` | Converts string to integer / double |
