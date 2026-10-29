# C++ Structures, Unions & Enums

These are user-defined data types in C++ that allow you to group, share, or represent data in meaningful ways.

---

# Part 1: Structures (`struct`)

A **`struct`** (structure) allows you to group multiple variables of **different data types** together under a single name.

> **Contrast with Arrays:**  
> * **Array:** Stores multiple items of the **same** type (`int arr[5]`).  
> * **Struct:** Stores multiple items of **different** types (`string`, `int`, `float`).

---

### 1. Declaring a Struct

```cpp
struct Student {
    int id;
    string name;
    float gpa;
}; // ⚠️ DON'T FORGET the semicolon at the end!
```

---

### 2. Creating & Initializing Struct Variables

#### Method A: Member-by-Member Assignment (Dot `.` Operator)
```cpp
Student s1;
s1.id = 101;
s1.name = "Alice";
s1.gpa = 3.85;

cout << s1.name << " has a GPA of " << s1.gpa;
```

#### Method B: List / Aggregate Initialization (C++11)
Values are assigned in the exact order members were declared:
```cpp
Student s2 = {102, "Bob", 3.50};
```

#### Method C: Designated Initializers (Modern C++20)
Explicit and very readable:
```cpp
Student s3 = {.id = 103, .name = "Charlie", .gpa = 3.92};
```

---

### 3. Pointers to Structs and the Arrow Operator (`->`)

When you have a pointer to a struct, you can access its members in two ways:

1. **Dereference then dot:** `(*ptr).name` (Parentheses are mandatory because `.` has higher precedence than `*`).
2. **Arrow operator (`->`):** `ptr->name` (Cleaner, standard shorthand!).

```cpp
Student s1 = {101, "Alice", 3.85};
Student* ptr = &s1;

// Both do the exact same thing:
cout << (*ptr).name; // Alice
cout << ptr->name;   // Alice (Preferred syntax! ⭐)

// Modifying values through pointer:
ptr->gpa = 4.0;
cout << s1.gpa;      // 4.0
```

---

### 4. Nested Structures

A struct can contain another struct as a member.

```cpp
struct Address {
    string city;
    int pinCode;
};

struct Person {
    string name;
    Address addr; // Nested struct
};

Person p1 = {"Sumit", {"New Delhi", 110001}};

cout << p1.name << " lives in " << p1.addr.city;
```

---

### 5. Passing Structs to Functions

* **By Value (Slow for large structs):** Copies the entire struct.
* **By Reference (Best practice ⭐):** Passes the memory address directly without copying. Use `const` if you don't need to modify it.

```cpp
// Efficient: No copying, read-only
void printStudent(const Student& s) {
    cout << "ID: " << s.id << ", Name: " << s.name << "\n";
}
```

> **Struct vs Class in C++:**  
> In C++, `struct` and `class` are almost identical. The **only** difference is default visibility:
> * `struct` members are **`public`** by default.
> * `class` members are **`private`** by default.

---

# Part 2: Unions (`union`)

A **`union`** is similar to a `struct`, **BUT all members share the exact same memory location**.

### 1. Memory Difference: `struct` vs `union`

```cpp
struct StructExample {
    int a;    // 4 bytes
    double b; // 8 bytes
    char c;   // 1 byte
}; // Total size ≈ 16 bytes (sum of members + padding)

union UnionExample {
    int a;    // 4 bytes
    double b; // 8 bytes
    char c;   // 1 byte
}; // Total size = 8 bytes (size of the LARGEST member: double)
```

```text
STRUCT MEMORY (Stacked side-by-side):
[ int a (4B) ] [ double b (8B) ] [ char c (1B) ] -> Total ~16B

UNION MEMORY (Overlapping at the same address):
[======= double b (8B) =======]
[ int a (4B) ]
[ char c (1B)]                                  -> Total 8B
```

---

### 2. ⚠️ The Golden Rule of Unions

> **A union can only hold ONE active value at any given time.**  
> Writing to one member **overwrites** all other members!

```cpp
union Data {
    int intVal;
    float floatVal;
    char charVal;
};

Data d;

d.intVal = 65;
cout << d.intVal << "\n"; // Output: 65

d.charVal = 'A';          // Overwrites the same memory!
cout << d.charVal << "\n"; // Output: A

// ❌ Trap: Reading intVal now produces undefined/unexpected data!
cout << d.intVal << "\n"; // May print garbage or unexpected bits
```

### When are Unions used?
* Low-level hardware programming & embedded systems with severe memory constraints.
* Type punning (interpreting raw bytes as different types).
* *(Modern C++17 alternative: use type-safe `std::variant` instead).*

---

# Part 3: Enumerations (`enum`)

An **`enum`** (enumeration) is a user-defined type that assigns **meaningful names to integer constants**.

Instead of using cryptic "magic numbers" (like `0`, `1`, `2`) for states or categories, enums make code self-documenting.

---

### 1. Traditional C-Style `enum`

By default, values start at `0` and increase by `1`:

```cpp
enum Direction {
    NORTH, // 0
    EAST,  // 1
    SOUTH, // 2
    WEST   // 3
};

Direction dir = EAST;

if (dir == EAST) {
    cout << "Heading East!";
}
```

#### Custom Integer Values:
You can assign specific numbers. Any unassigned subsequent member increments from the previous one:

```cpp
enum HttpStatus {
    OK = 200,
    BAD_REQUEST = 400,
    NOT_FOUND = 404,
    INTERNAL_ERROR = 500
};

HttpStatus status = NOT_FOUND;
cout << status; // Output: 404
```

---

### 2. Clean `switch` with `enum`
Enums pair perfectly with `switch` statements:

```cpp
enum TrafficLight { RED, YELLOW, GREEN };

TrafficLight light = RED;

switch (light) {
    case RED:
        cout << "Stop!";
        break;
    case YELLOW:
        cout << "Caution!";
        break;
    case GREEN:
        cout << "Go!";
        break;
}
```

---

### 3. ⭐ Modern C++: Scoped Enums (`enum class`)

Traditional enums have two major flaws:
1. **Namespace Pollution:** Names leak into the surrounding scope (e.g., you cannot have two different enums with a member named `RED`).
2. **Implicit Conversion to `int`:** You can accidentally compare `Color::RED == TrafficLight::RED` or `Status == 1`.

C++11 introduced **`enum class`** (Scoped Enum) to solve this:

```cpp
enum class Color {
    Red,
    Green,
    Blue
};

enum class TrafficLight {
    Red,
    Yellow,
    Green
};

// 1. Must use scope resolution (Color::)
Color c = Color::Red;
TrafficLight t = TrafficLight::Red;

// 2. Safe: Cannot accidentally compare them!
// if (c == t) -> ❌ COMPILE ERROR! Types don't match.

// 3. No implicit int conversion:
// int x = c; -> ❌ COMPILE ERROR!
int x = static_cast<int>(c); // ✅ Must explicitly cast if needed
```

---

# Summary Comparison Table 📋

| Feature | `struct` | `union` | `enum` / `enum class` |
| :--- | :--- | :--- | :--- |
| **Purpose** | Group related variables of different types | Store multiple types in the **same** memory spot | Represent a set of named integer constants |
| **Memory Allocation** | Sum of all members (+ padding) | Size of the **largest member** | Size of an integer (usually 4 bytes) |
| **Simultaneous Values** | All members can hold values at the same time | **Only one** member holds a valid value at a time | Only holds one named constant value |
| **Access Operator** | Dot (`.`) or Arrow (`->`) | Dot (`.`) or Arrow (`->`) | Scope resolution (`::`) |
