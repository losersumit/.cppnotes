# C++ Notes

A collection of beginner-friendly C++ notes and examples.

## Table of Contents

- [Classes and Objects](#classes-and-objects)
  - [What is a class?](#what-is-a-class)
  - [What is an instance?](#what-is-an-instance)
  - [Creating objects and using members](#creating-objects-and-using-members)
  - [Using object data inside member functions](#using-object-data-inside-member-functions)
  - [Access specifiers and encapsulation](#access-specifiers-and-encapsulation)
  - [Constructors](#constructors)
  - [The `this` pointer](#the-this-pointer)
  - [Objects and memory](#objects-and-memory)
  - [Passing objects to functions](#passing-objects-to-functions)
  - [Returning objects from functions](#returning-objects-from-functions)
  - [Const member functions](#const-member-functions)
  - [Static members](#static-members)
  - [A complete example: 2D vector](#a-complete-example-2d-vector)
  - [Common mistakes](#common-mistakes)

---

# Classes and Objects

C++ lets you define your own types with **classes**. A class groups data and the functions that work on that data.

Think of a class as a blueprint and an object as one specific thing built from it. For example, `Student` can be a class, while `student1` and `student2` are two separate objects (instances) of that class.

## What is a class?

A **class** is a user-defined type. It specifies the data members and member functions its objects have.

```cpp
#include <iostream>
#include <string>

class Student {
public:
    std::string name;
    int rollNumber;

    void introduce() {
        std::cout << "Hi, I am " << name << '\\n';
    }
};
```

Important syntax:
- `class` begins the class definition.
- `Student` is the class name.
- The body is enclosed by braces.
- A semicolon **must** follow the closing brace: `};`.
- Defining the class creates a type; it does not itself create a `Student` object.

## What is an instance?

An **instance** is an individual object created from a class. In beginner C++ discussions, *object* and *instance of a class* are often used interchangeably.

```cpp
Student student1; // student1 is an instance of Student
Student student2; // student2 is another instance of Student

student1.name = "Asha";
student1.rollNumber = 12;

student2.name = "Rahul";
student2.rollNumber = 15;

student1.introduce(); // Hi, I am Asha
student2.introduce(); // Hi, I am Rahul
```

Both objects follow the same class definition, but each has its own `name` and `rollNumber`. Changing `student1.name` does not change `student2.name`.

## Creating objects and using members

Declare an object using the class name, followed by the object's name:

```cpp
ClassName objectName;
```

If the class has a suitable constructor, you can initialize the object while creating it:

```cpp
Point p1(3, 4);
Point p2{5, 2};
```

Use the **dot operator** (`.`) to access public members:

```cpp
objectName.dataMember = value;
objectName.memberFunction();
```

For example, `student1.introduce()` calls the function for `student1`. The object before the dot determines which object's data the member function works with.

## Using object data inside member functions

A non-static member function can directly use the data members of the object on which it is called. You do not have to pass each member variable as a separate argument.

```cpp
#include <iostream>

class Vector {
public:
    int x;
    int y;

    void set(int a, int b) {
        x = a; // store the arguments in this object's members
        y = b;
    }

    int sum() {
        return x + y; // read this object's data
    }

    void display() {
        std::cout << "(" << x << ", " << y << ")\\n";
    }
};

int main() {
    Vector v1;
    Vector v2;

    v1.set(3, 4);
    v2.set(10, 20);

    v1.display(); // (3, 4)
    v2.display(); // (10, 20)

    std::cout << v1.sum() << '\\n'; // 7
    std::cout << v2.sum() << '\\n'; // 30
}
```

**What happens here?**
- `v1` and `v2` are two instances of `Vector`.
- `v1.set(3, 4)` stores `3` in `v1.x` and `4` in `v1.y`.
- `v2.set(10, 20)` changes only `v2`'s members.
- When you call `v1.sum()`, it calculates `v1.x + v1.y`. When you call `v2.sum()`, it calculates `v2.x + v2.y`.

The function parameters `a` and `b` are local variables. The assignments `x = a` and `y = b` copy their values into the current object's data members.

## Access specifiers and encapsulation

Access specifiers control where members can be accessed.

| Specifier | Meaning |
| --- | --- |
| `public` | Accessible through the object's public interface. |
| `private` | Accessible from member functions and friends of the class. |
| `protected` | Accessible from the class, its friends, and derived classes. |

In a `class`, members are **private by default**. In a `struct`, members are **public by default**.

```cpp
class BankAccount {
private:
    double balance = 0.0;

public:
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    double getBalance() const {
        return balance;
    }
};
```

Because `balance` is private, code outside the class cannot write `account.balance = -100;`. It must use the public functions. Keeping data and the operations that manage it together is called **encapsulation**.

## Constructors

A **constructor** runs when an object is created. It has the same name as the class and no return type—not even `void`.

### Default constructor

```cpp
class Point {
public:
    double x;
    double y;

    Point() : x(0), y(0) {}
};

Point origin; // origin.x == 0 and origin.y == 0
```

### Parameterized constructor

```cpp
class Point {
public:
    double x;
    double y;

    Point(double xValue, double yValue)
        : x(xValue), y(yValue) {}
};

Point p(3, 4);
Point q{5, 2};
```

The part after the colon is the **member-initializer list**. It initializes members directly. A constructor can be overloaded with different parameter lists. If you declare a constructor, C++ does not automatically provide the implicit default constructor, so add one if you need it.

## The `this` pointer

Inside a non-static member function, `this` points to the object on which the function was called. It can distinguish a member from a parameter with the same name.

```cpp
class Point {
private:
    double x;

public:
    Point(double x) : x(x) {}

    void setX(double x) {
        this->x = x;
    }
};
```

In `this->x = x;`, `this->x` is the object's member and the right-hand `x` is the parameter.

## Objects and memory

Each ordinary non-static data member belongs to its own object:

```cpp
Point a{1, 2};
Point b{10, 20};
```

The coordinates in `a` are separate from the coordinates in `b`. Member functions operate on the object they are called on. The exact size of an object may include padding for memory alignment, so `sizeof(object)` is not necessarily just the sum of its member sizes.

## Passing objects to functions

You can pass an object to an ordinary function in several ways:

```cpp
#include <iostream>

class Point {
public:
    double x, y;
    Point(double x, double y) : x(x), y(y) {}
};

void printPoint(const Point& p) {
    std::cout << "(" << p.x << ", " << p.y << ")\\n";
}

void moveRight(Point& p) {
    p.x += 1; // modifies the original object
}

int main() {
    Point p{3, 4};
    printPoint(p); // reads it without copying
    moveRight(p);
    printPoint(p); // (4, 4)
}
```

- `Point p`: pass by value; the function gets a copy.
- `Point& p`: pass by reference; the function can modify the original.
- `const Point& p`: pass by const reference; the function can read the object without copying it, but cannot modify it through that reference.

The example uses public coordinates so the free functions can access them. If members are private, expose an appropriate public interface.

## Returning objects from functions

A function can return an object, just as it can return an `int` or a `double`.

```cpp
class Point {
public:
    double x, y;
    Point(double x, double y) : x(x), y(y) {}
};

Point shiftedRight(const Point& p) {
    return Point(p.x + 1, p.y);
}

// Example use:
// Point p{3, 4};
// Point q = shiftedRight(p); // q is (4, 4); p is unchanged
```

This is useful for operations that produce a new value instead of modifying an existing object.

## Const member functions

Put `const` after a member function's parameter list when it should not modify the object's ordinary state:

```cpp
double length() const {
    return std::sqrt(x * x + y * y);
}
```

A const object can call const member functions, but generally cannot call non-const member functions. This helps catch accidental modifications.

## Static members

A `static` data member belongs to the class rather than to each object. All objects share it.

```cpp
class Counter {
public:
    inline static int total = 0; // C++17 or later

    Counter() {
        ++total;
    }
};

Counter a;
Counter b;
std::cout << Counter::total; // 2
```

The scope operator (`::`) accesses a static member through the class name.

---

## A complete example: 2D vector

A mathematical **2D vector** is represented by two components, `x` and `y`. For example, `(3, 4)` has horizontal component `3` and vertical component `4`. This is a custom mathematical vector class, not the C++ Standard Library container `std::vector<T>`.

### Complete program

```cpp
#include <iostream>
#include <cmath>

class Vector2D {
private:
    double x;
    double y;

public:
    // Constructor: defaults make Vector2D() the zero vector.
    Vector2D(double xValue = 0.0, double yValue = 0.0)
        : x(xValue), y(yValue) {}

    // Getter functions provide read access to private data.
    double getX() const { return x; }
    double getY() const { return y; }

    // Set both components.
    void set(double xValue, double yValue) {
        x = xValue;
        y = yValue;
    }

    // Add another vector and return a new object.
    Vector2D add(const Vector2D& other) const {
        return Vector2D(x + other.x, y + other.y);
    }

    // Multiply both components by a scalar.
    Vector2D scaled(double factor) const {
        return Vector2D(x * factor, y * factor);
    }

    // Magnitude = sqrt(x^2 + y^2).
    double magnitude() const {
        return std::sqrt(x * x + y * y);
    }

    void display() const {
        std::cout << '(' << x << ", " << y << ')';
    }
};

int main() {
    // v1, v2, sum, and doubled are instances of Vector2D.
    Vector2D v1(3, 4);
    Vector2D v2(1, 2);

    std::cout << "v1 = ";
    v1.display();

    std::cout << "\\nv2 = ";
    v2.display();

    Vector2D sum = v1.add(v2);
    std::cout << "\\nv1 + v2 = ";
    sum.display();

    Vector2D doubled = v1.scaled(2);
    std::cout << "\\n2 * v1 = ";
    doubled.display();

    std::cout << "\\nMagnitude of v1 = " << v1.magnitude() << '\\n';

    // Read private components through public getter functions.
    std::cout << "x component of v1 = " << v1.getX() << '\\n';

    return 0;
}
```

### Output

```text
v1 = (3, 4)
v2 = (1, 2)
v1 + v2 = (4, 6)
2 * v1 = (6, 8)
Magnitude of v1 = 5
x component of v1 = 3
```

### Understanding the example

1. **Define the class:** `class Vector2D { ... };` defines the type and its members.
2. **Create instances:** `Vector2D v1(3, 4);` creates an object whose components are initialized to `3` and `4`. `v2` is a separate instance.
3. **Use member functions:** `v1.display()` calls the function for `v1`. The function accesses `v1`'s own `x` and `y`.
4. **Pass an object to a member function:** `v1.add(v2)` calls `add` on `v1`; the parameter `other` refers to `v2`. Inside `add`, `x` and `y` refer to `v1`'s components, while `other.x` and `other.y` refer to `v2`'s components. A member function can access private members of another instance of the same class.
5. **Return a new instance:** `add()` returns `Vector2D(x + other.x, y + other.y)`. This makes a new result and leaves `v1` and `v2` unchanged.
6. **Use private data safely:** `v1.getX()` returns the x component. Direct access such as `v1.x` is not allowed because `x` is private.
7. **Calculate magnitude:** `sqrt(3² + 4²) = 5`.

### Vector addition

For `a = (a_x, a_y)` and `b = (b_x, b_y)`:

`a + b = (a_x + b_x, a_y + b_y)`

So `(3, 4) + (1, 2) = (4, 6)`.

### Magnitude

For `v = (x, y)`:

`|v| = sqrt(x² + y²)`

For `v = (3, 4)`, the magnitude is `sqrt(3² + 4²) = sqrt(25) = 5`.

## Common mistakes

- **Forgetting the semicolon:** end a class definition with `};`.
- **Giving a constructor a return type:** constructors have no return type.
- **Confusing class and instance:** `Vector2D` is the class/type; `v1` is an instance/object.
- **Using the wrong object:** `v1.display()` acts on `v1`, while `v2.display()` acts on `v2`.
- **Accessing private data directly:** use public member functions, such as getters, instead of `v1.x` when `x` is private.
- **Confusing parameters and members:** in `set(int x)`, `x` is a parameter; `this->x` identifies the member.
- **Confusing mathematical vectors with `std::vector`:** a `Vector2D` represents a 2D vector; `std::vector<T>` is a dynamically sized sequence.

## Quick recap

- A **class** defines a user-defined type.
- An **object/instance** is a value created from that class.
- **Data members** store state; **member functions** use or modify that state.
- Use `object.member` to access public members.
- **Constructors** initialize new objects.
- **References** let functions work with existing objects; `const` references are useful for read-only access.
- **Encapsulation** protects internal data behind a public interface.
- A **2D vector** demonstrates how a class combines data (`x`, `y`) and behavior (addition, scaling, magnitude).
