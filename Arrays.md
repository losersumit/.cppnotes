# C++ Arrays

An array is a collection of elements of the **same data type** stored in contiguous memory locations.

---

## 1. Declaring and Initializing Arrays

### Syntax
```cpp
dataType arrayName[arraySize];
```
 
### Examples of Initialization
```cpp
// 1. Declare and initialize size, but assign values later
int numbers[5]; 

// 2. Initialize with elements (compiler automatically determines size as 5)
int grades[] = {90, 85, 98, 77, 92}; 

// 3. Declare size and initialize elements
string cars[4] = {"Volvo", "BMW", "Ford", "Mazda"};
```

---

## 2. Accessing & Modifying Elements
Arrays are **0-indexed** (the first element is at index `0`).

```cpp
string cars[4] = {"Volvo", "BMW", "Ford", "Mazda"};

// Accessing elements
cout << cars[0]; // Output: Volvo
cout << cars[2]; // Output: Ford

// Modifying elements
cars[0] = "Opel";
cout << cars[0]; // Output: Opel
```

---

## 3. Looping through an Array

### Using a standard `for` Loop
```cpp
int grades[] = {90, 85, 98, 77, 92};

for (int i = 0; i < 5; i++) {
    cout << grades[i] << " ";
}
```

### Using a Range-based `for` Loop (C++11)
```cpp
int grades[] = {90, 85, 98, 77, 92};

for (int grade : grades) {
    cout << grade << " ";
}
```

---

## 4. Getting Array Size
To find out how many elements an array has, you can divide the total memory size of the array by the size of one element.

```cpp
int numbers[] = {10, 20, 30, 40, 50};

int arraySize = sizeof(numbers) / sizeof(numbers[0]);
cout << arraySize; // Output: 5
```

*(Note: In modern C++17, you can use `std::size(numbers)` by including `<iterator>`)*

---

## 5. Multidimensional Arrays (2D Arrays)
An array of arrays. Often used to represent grids or matrices.

### Declaration and Initialization
```cpp
// Row x Column matrix (2 rows, 3 columns)
int matrix[2][3] = {
    {1, 2, 3},
    {4, 5, 6}
};
```

### Accessing & Modifying 2D Arrays
```cpp
cout << matrix[0][2]; // Output: 3 (Row 0, Col 2)

matrix[1][0] = 9; // Changes 4 to 9
```

### Looping through a 2D Array
```cpp
for (int i = 0; i < 2; i++) {       // Outer loop for rows
    for (int j = 0; j < 3; j++) {   // Inner loop for columns
        cout << matrix[i][j] << " ";
    }
    cout << "\n";
}
```

---

## 6. ⚠️ Critical Pitfalls

### 1. Out of Bounds Access
C++ does not check array boundaries at runtime. Accessing an index outside the size of the array leads to **Undefined Behavior** (crashing, reading garbage values, or memory leaks).
```cpp
int num[3] = {1, 2, 3};
cout << num[5]; // ❌ Out of bounds!
```

### 2. Fixed Size at Compile-Time
Traditional arrays must have a size determined at compile-time. You cannot use a variable to set the size unless it is `const` or `constexpr`.
```cpp
int size = 5;
int arr[size]; // ❌ Incorrect in standard C++ (though some compilers allow VLA extension)

const int correctSize = 5;
int arr2[correctSize]; // ✅ Correct
```

---

## 7. Arrays and Pointers (Decay & Arithmetic)

An array's name acts as a pointer to its first element (`arr[0]`):

```cpp
int numbers[] = {10, 20, 30};
int* ptr = numbers; // ptr points to numbers[0]

cout << *ptr;       // 10
cout << *(ptr + 1); // 20 (moves by sizeof(int) bytes)
```

👉 *Array indexing `numbers[i]` is actually shorthand for `*(numbers + i)`.*  
*(For full details on pointer arithmetic and hex addresses, see [Pointers.md](file:///s:/C++/Notes/Pointers.md))*

