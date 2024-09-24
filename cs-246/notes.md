# Introduction

Object-oriented programming will be looked at in 3 perspectives:

1. **Programmer**: How to structure programs to reduce the risk of bugs?
2. **Compiler**: What do our constructions mean and how does our compiler support them?
3. **Designer**: How to use OOP tools to build systems?

## C vs. C++

| C                                                   | C++                                                                                                          |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| _#include_ used for imports                         | _import_ used for imports using new module system                                                            |
| main() function may not return an int `void main()` | main() function must return an int `int main()`                                                              |
| N/A                                                 | C++ organizes names into namespaces to prevent name collisions. A common namespace is ``using namespace std` |

## Types and Sizes

- bool: 8 bits
- char: 8 bits
- short: 16 bits
- int: 32 bits
- long: 64 bits
- long long: 64 bits
- float: 32 bits
- double: 64 bits
- long double: 128 bits
- unsigned char: 8 bits
- unsigned short: 16 bits
- unsigned int: 32 bits
- unsigned long: 64 bits
- unsigned long long: 64 bits

## Strings

C++ character strings are easy and safe to use, unlike _char\*_ from C, which is a source of many bugs and security vulnerabilities. Furthermore, there is no need to keep track of the trailing _\0_ character.

To use strings in C++, import the string library

```cpp
import <string>;
```

C++ allows _argc_ and _argv_ arguments to be passed to the _main_ program.

## Imports vs. Includes

With _includes_, the C preprocessor will textual include other C++ code from modules/libraries before compilation.

With _imports_, the C++ compiler handles the inclusion of C++ modules/libraries. Precompilation is required before the compiling of the main program, which may cause some issues when programs are split into multiple files.

## Simple I/O

C++ has 2 built-in output streams;

1. `cout` Output stream for writing to the standard output (stdout).
2. `cerr` Output stream for writing to standard error (stderr).

and an input stream, `cin`, for reading from stdin, the following rules are followed:

- **Enter** will submit the text to the program.
- **Ctrl-D** will signal the end of a file/input.
- All leading whitespaces such as tabs, newlines and spaces are ignored.
- Invalid inputs will cause the input operation to fail.
  - If the read fails, the `cin.fail()` flag will reture **TRUE**.
  - If it is the end of a file, both the `cin.fail()` and `cin.eof()` flags return true.
  - To skip invalid inputs, `cin.clear()` is used to clear the stream's failure flag and `cin.ignore([count],  [char])` is used to remove the invalid input from the stream.
    - The `count` parameter tells the function to skip `count` characters; often set to _INT_MAX_.
    - The ` char`` parameter tells the function to skip all characters until a  `char``` character is reached.

```cpp
import <iostream>;
```

The `<<` operator is used when writing/inserting to the output stream, and the `>>` operator is used when getting/extracting from the input stream.

Output/input can be formatted for better data representation.

```cpp
import <iomanip>;
using namespace std;

cout << hex << 95 << endl; // Converts 95 to hexadecimals
cout << dec << 5f << endl; // Converts 5f to decimal

double m = 10.9
cout << setw(10) << setfill('*') << right << fixed << setprecision(2) << m << endl; // Round to the 2nd decimal place, tries to round the number, set adjustfield format flag, reformat unfilled characters in input with '*' and reformat input to contain 10 characters.
```

## Strings

| C                                                  | C++                            |
| -------------------------------------------------- | ------------------------------ |
| `char*` or `char[]` with null character at the end | Has a built-in string datatype |
| Explicitly manage memory                           | Intuitive and easy to use      |
| Easy to overwrite _\0_ and corrupt memory          | Grows as needed                |
| Big source of security vulnerabilities             | Safer to manipulate            |

Common algebraic operators can be used on strings for equality comparisons and lexicographic comparisons, such as ==, !=, < and >.

```cpp
string s;
cin >> s; // Skips leading whitespace and stops at 1st whitespace.

string s;
getline(cin, s); // Reads from current position to the next \n, which is not included.

while (cin.get(c)) {
    cout << c; // Reads one char at a time.
}
```

## Streams

### File Streams

File streams are sequences of bytes used to hold file data, which read/write from a file instead of stdin/stdout. The two main streams are:
- ```ifstream``` which is for reading data.
- ```ofstream``` which is for writing data.

```cpp
import <fstream>;
import <iostream>;
using namespace std;

int main() {
    ifstream f { "file.txt" } // Opens file and closes it later.
    string s;
    while (f >> s) {
        cout << s << endl;
    }
}
```

### String Streams

String streams allow you to read from and write to strings using the stream type operations. The two main streams are:
- ```isstringstream``` which is for reading chars from a string.
- ```osstringstream``` which is for writing chars into a string.

```cpp
import <sstream>;
```

**Example** Integer to string

```cpp
string intToString(int n) {
    osstringstream oss; // Empty string stream that writes to a string
    oss << n;
    return oss.str(); // Extracts the string and returns it
}
```

**Example** Input a number and repeat until success

```cpp
int main() {
    while (true) {
        cout << "Enter a number: ";
        string s;
        cin >> s;
        istringstream iss { s };
        int n;
        if (iss >> n) {
            break;
        } else {
            cout << "Error! Not a number! Try again!";
        }
    }
}
```

**Example** Echo all numbers and stop at the first non-number

```cpp
int main() {
    string s;
    while (cin >> s) { // Loops until end of file.
        istringstream iss { s };
        int n;
        if (iss >> n) {
            cout << n << endl;
        }
    }
}
```

## Command-Line Arguments

We can access command-line arguments in a C++ program as follows:

```cpp
int main(int argc, char* argv[]) {
    // argc is the number of command-line arguments
    // argv[] is the list of command-line arguments
}
```

Each element in argv is a _char\*_, a pointer to a C-style string, which includes a null terminator at the end of the string. The length of argv is actually _argc + 1_, with the last element being **null**.

To convert the list of C-style strings into C++-style strings.

```cpp
for (int i = 1; i < argc; ++i) { // Skipping first argument which is program name
    string arg = argv[i];
    cout << arg << endl;
}
```

**Example** Print the sum of all command-line arguments that are numeric

```cpp
int main(int argc, char* argv[]) {
    int sum = 0;
    for (let i = 1; i < argc; ++i) {
        string arg { argv[i] };
        istringstream iss { arg };
        if (iss >> n) {
            sum += n;
        }
    }
    cout << sum << endl;
}
```

## Default Function Parameters

C++ allows you to define a function with default or optional parameters. Optional parameters must be last in the function signature. The missing parameters are supplied by the caller and not the function, since the compiler re-writes the code as if it were being passed with the default parameter value.

This is because, when a function is called and placed on the stack, the function retrives the parameters from the stack. If a parameter is missing, then the function would read some random data, causing errors.

```cpp
void printSuiteFile(string name = "suite.txt") { // Default value
    ifstream file { name }
    for (string s; file >> s;) { // Reads the file one word at a time
        cout << s << endl;
    }
}
```

If a function is split into an interface and implementation file, the default function parameters must be specified in the interface file.

```cpp
// interface.h
void printSuiteFile(string name = "suite.txt");

// implementation.cc
void printSuiteFile(string name) {
    // Code...
}
```

## Overloading

**Overloading** is when the same function/operator has multiple implementations and the compiler chooses the correct implementation (at compile time) based on the number of parameters and the types of parameters.

Note that the parameter of a function is a formal name for receiving an argument, which is what is passed.

In C, a common implementation for a negate function would be as such:

```c
int negateInt(int n) {
    return -n;
}

bool negateBool(bool b) {
    return !b;
}
```

In C++, using overloading, a two functions with the same name can be written:

```cpp
int negate(int n) {
    return -n;
}

bool negate(bool b) {
    return !b;
}
```

The compiler chooses the correct version of _negate_ for each function based on the number and types of arguments at compile time. For example:

```cpp
negate(5); // Compiler chooses int negate(int n) {}
negate(true); // Compiler chooses bool negate(bool b) {}
```

Situations where two functions with the same name have:

- The same number and type of parameters with different return types
- The same number and type of parameters excluding optional parameters
- The same number and type of parameters altogether

Would cause compiler errors.

## Structs

Structs are a way to group several variables together.

```cpp
struct Date {
    int year;
    int month;
    int day;
};

// Must initialize variables in struct in declaration order.

Date day1 = { 2024, 9, 12 };
Date day2 { 2024, 9, 13 };
Date day3; // Values in the fields of the struct could be random.
```

**Example**

```cpp
// Node struct for linked list.
struct Node {
    int data;
    Node *next;
}

// Compilation error since next is not fully initialized/defined.
struct Node {
    int data;
    Node next;
}
```

## Constants

C++ support variables that cannot be changed after being intialized, which are declared using the _const_ keyword.

```cpp
const int maxGrade = 100;
const Node n1 { 1, nullptr }; // nullptr is a keyword used for null pointers in C++
n1.data++; // Invalid
Node n2 = n1;
n2.data++; // Valid
```

Declare as many things _const_ as possible, as it helps catch programming errors when you inadvertantly change variables.

## Parameters

```cpp
void inc(int n) { n++; }
int x = 5;
inc(x); // Pass by value
cout << x << endl;
```

```cpp
void inc(int *n) { (*n)++; }
int x = 5;
inc(&x); // Pass by reference/pointer; the address itself is passed by value
cout << x << endl;
```

## References

The following functions seem to pass the variables by value, but in reality, they are passing by reference, allowing the variables to be updated after the function is ran.

```cpp
string s;
getline(cin, s);

char c;
cinget(c);

int n;
cin >> n;
```

This is done using _references_, which allows us to have multiple names (aliases) that refer to the same object. Any changes to the reference(s) of a variable reflect to that variable. References can be thought of as _const pointers_ with _automatic dereferencing_.

```cpp
int y; // Declares variable that allocates space; y is an object that can store an int.
y = 10;
int &z = y; // z is a reference to the memory location which stores y.
z = 12;
cout << y // 12
```

```cpp
int arr[3] = {10, 20, 30};
int &i1 = arr[1];
cout << i1; // 20
i1 += 5;
cout << arr[1] // 25
```

**&** has been the "address of" operator. Whenever **&** appears as part of a type, it is being used to create a reference/alias. Whenever it occurs in an expression, it is being used to get the address of a variable.

Invalid operations with references include:

- Not initializing a reference `int &z;`
- Initializing a reference with something that is not an address `int &z = x + y; int &z = 3`
- Changing a reference to reference something else; references are constants `int x = 10, y = 11; int &z = x; z = y`. In the last statement, the compiler evaluates it as `x = y`. Similarly, `int x = 10; int &z = x; int &a = z;` would have a last expression that evaluates to `a = x`
- Creating a pointer to a reference `int &*z;`. However, a reference to a pointer is valid `int x = 10; int *xp = &x; int *&z = xp;`
- Creating a reference to a reference `int x = 10; int &z = x; int &&zz = z;`. This actually means something else in C++.
- Creating an array of references `int &arr[3] = {x, y, z};`

```cpp
void inc(int &n) { n++; } // Pass by reference
int x = 5;
int y = 10;
inc(x); // n becomes a reference to x
inc(y); // n becomes a reference to y
cout << x << "," << y << endl; // 6,11
```

```cpp
void swap(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}

int x = 5, y = 10;
swap(x, y); // x and y is passed by reference as a and b.
cout << x << "," << y << endl; // 10,5
```

```cpp
int f(int &n) {...};
int g(const int &n) {...};
f(5); // Invalid
g(5); // Valid
```

### By Value vs. By Reference

When passing a really big variable, passing by **value** will be expensive in terms of CPU and memory.

```cpp
struct ReallyBig {
    ...
};

// Pass by value
int f1(ReallyBig rb) { ... };
// Pass by reference
int f2(ReallyBig &rb) { ... };
int f3(const ReallyBig &rb) { ... };
```

| Function | Copied? | Fast/Slow | Changes Visible to Caller?  |
| -------- | ------- | --------- | --------------------------- |
| f1       | Yes     | Slow      | No                          |
| f2       | No      | Fast      | Yes                         |
| f3       | No      | Fast      | Yes (changes can't be made) |

In general,

- Prefer passing by reference over passing by value for anything larger than a pointer, allowing for faster parameter passing.
- If the function won't change the parameter, then add _const_ to the parameter, telling the caller of the function that the parameter won't be changed and preventing the function from changing the parameter.
- If you need to make changes that shouldn't be visible to the caller, create a local copy of the parameter.

For instance, the `>>` operator is implemented as a regular C++ function with references:

```cpp
// cin >> x; => operator>>(cin, x);

istream &operator>> (istream &in, int &n) {
    // Reads characters for the stream, interprets as an integer and puts the value into n.
    return in;
}
```

**Overloading** is used in this case for different input types, such as integer, string, etc.

## Dyanmic Memory Allocation

In C, we will use the _malloc_ function to allocate memory.

```c
int *p = malloc(sizeof(int) * 10);
p[0] = 10;
p[1] = 20;
free(p);
```

In C++, don't use _malloc_ or _free_ functions; rather, use _new_ and _delete_ functions, which are more robust and less error-prone.

```cpp
int *p = new int; // Creates an int object on the heap and returns a pointer to it.
*p = 10;
delete p; // Removes the int object from the heap; p is now a dangling pointer, with an address pointing to memory which stores nothing.
```

```cpp
Node *np = new Node;
np->data = 42; // Or (*np).data = 42;
delete np;
np = nullptr; // To prevent dangling pointers
```

The main memory stores data such as:
- OS and other programs
- Our program
- Memory allocated to our program, which includes static data, the heap and the stack.

The stack stores data such as _function parameters_ and _local variables_, which are deallocated automatically once the stack frame for that data is popped/removed. For pointers, the memory that they point to isn't deallocated automatically and resides on the heap until _delete_ is called. Failure to do so results in a memory leak.

Calling _delete_ more than once, such as on a dangling pointer, resulting in an error. Calling _delete_ on a ```nullptr``` is okay to do. Never call _delete_ on a stack-allocated object, since the system already manages the deallocation of stack-allocated objects.

```cpp
Node n;
Node *np = &n; // Points to stack-allocated object
delete np; // Invalid
```

### Dynamic Array Allocation

To dynamically allocate an array, we use the _new[]_ and _delete[]_ functions rather than the _new_ and _delete_ functions, which creates an array of **n** elements on the heap which are stored back-to-back and returns the pointer to the first element.

```cpp
Node *nodeArray = new Node[10];
delete[] nodeArray;

int *intArray = new int[10];
delete[] intArray;
delete intArray; // Invalid
```

```cpp
int arraySum(int *intPtr, int size) {
    int sum = 0;
    while (size > 0) {
        sum += *intPtr;
        intPtr++; // Moves to the next pointer
        --size;
    }
    return sum;
}

int arraySum(int *intPtr, int size) {
    int sum = 0;
    for (int i = 0; i < size; ++i) {
        sum += intPtr[i];
    }
    return sum;
}

int arraySum(int *intPtr, int size) {
    int sum = 0;
    for (int *p = intPtr; p < intPtr + size; ++p) {
        sum += *p;
    }
    return sum;
}
```

## Returning Values

There are 3 ways to return a value from a function, which include:

**Return by value** is when a value is returned from a function to the caller before the function's stack frame is popped/removed.

```cpp
Node getMeANode() {
    Node n;
    ...
    return n;
}
Node n1 = getMeANode();
```

**Return by pointer** is when the pointer to a value is returned from a function to the caller. However, this only works if the pointer returned points to heap-allocated memory. If it points to stack-allocated memory, whenever the function's stack frame is popped/removed, the data which the returned pointer points to is deleted.

```cpp
// Stack-allocated Node
Node *getMeANode() {
    Node n;
    ...
    return &n;
}
Node *n1 = getMeANode();


// Heap-allocated Node
Node *getMeANode() {
    Node *np = new Node;
    return np;
}
Node *n1 = getMeANode(); // Transfers ownership to caller; must be deleted to prevent memory leaks
```

**Return by reference** is when a reference to a value is returned from a function to the caller. However, this almost never works, since popping/removing the function's stack frame deletes the data which the returned reference/alias refers to.

```cpp
Node &getMeANode() {
    Node n;
    ...
    return n;
}
Node &n1 = getMeANode();
```

_Return by value_ is preferred over _return by pointer_ since it is quite fast and efficient due to compiler design and optimizations which prevent returning values to depend on the size of the object.

## Operator Overloading

Operator overloading allows us to use the built-in C++ operators with the user-defined types we create.

**Example**

```cpp
struct Vect {
    int x, y;
};

Vec operator+(const Vec &v1, const Vec &v2) { // Reference to prevent copying of arguments which occurs in pass by value
    Vec v {v1.x + v2.x, v1.y + v2.y};
    return v;
}

Vec operator*(const Vec &v, const int k) {
    return {v.x * k, v.y * k}; // Compiler knows we are returning a Vec
}

Vec v1 {1, 2};
Vec v2 {4, 5};

Vec v3 = v1 + v2; // Calls the Vec operator+ function
Vec v4 = v1 * 3; // Calls the Vec operator* function
Vec v5 = (v1 + v2) * 3;
Vec v6 = 10 * v1; // Invalid; order matters

Vec operator*(const int k, const Vec &v) {
    return v * k;
}

Vec v6 = 10 * v1; // Valid; order doesn't matter anymore
```

We can also overload the input ```>>``` and output ```<<``` operators.

```cpp
// Returning by reference in these cases work since we are returning the same reference value that is being passed to the function

ostream operator<<(ostream &out, const vec &v) {
    out << "(" << v.x << "," << v.y << ")";
    return out;
}

istream operator>>(istream &in, vec &v) {
    char p1, p2, c;
    int x, y;
    in >> p1 >> x >> c >> y >> p2;

    // The input stream fails when the input can't be assigned to the integer
    if (!in.fail() && p1 == '(' && p2 == ')' && c == ',') {
        v.x = x;
        v.y = y;
    }
    return in;
}    
```

## Modules & Separate Compilation

A C++ program can be split into modules where each module provides an **interface**, which includes type defintions, function prototypes, etc., and an **implementation**, which includes the full definitions for every provided function.

The naming convention for modules files is assigning the name of the module, ```name``` to the _interface_ file and ```name-impl``` to the _implementation_ file.

**Example**

```cpp
// Interface (vector.cc)

export module vector; // Assigns a name to the module and indicates this file is the module interface file; must be at the top of the file

// Any block of code with export preceding it will be exposed/exported from the module.

export struct Vec {
    int x, y;
};

export Vec operator+(const Vec &v1, const Vec &v2);
export Vec operator*(const Vec &v1, const int k);
...
```

```cpp
// Implementation (vector-impl.cc)

module vector; // Indicates this file is part of the module vector and implicitly imports the interface file; must be at the top of the file

Vec operator+(...) {
    ...
}

Vec operator*(...) {
    ...
}
```

```cpp
// Client (main.cc)

import vector; // No angle brackets for user modules; must be at the top of the file
```

Function implementations may be defined in the _interface_ file altogether rather than creating a separate _implementation_ file. However, it is not good practice.

These files can include non-exported types, functions, etc., such as helpers functions in the _implementation_ file which aren't exported.

To compile a user module, the following order of compilation commands should be used:

```bash
g++20 -c {name}.cc # Compiles the module into gcm.cache
g++20 -c {name}-impl.cc
g++20 -c main.cc

# The -c flag compiles the module to a .o file only without linking or creating an executable

g++20 {name}-impl.o main.o -o main # Links and creates a executable file 'main' with the compiled module and non-module files
```

Benefits of modules include:
- Faster compilation
- Modules can use other modules as part of their implementation without exposing them to the client
- Can be imported in any order
- You can use modules and header files together