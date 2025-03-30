# C++ Elements Quick Reference

## Table of Contents
- [Variables and Data Types](#variables-and-data-types)
- [Operators](#operators)
- [Control Structures](#control-structures)
- [Arrays](#arrays)
- [Functions](#functions)
- [String Operations](#string-operations)

## Variables and Data Types
```cpp
// Primitive Data Types
int number = 42;                  // Integer values
double decimal = 3.14;            // Decimal numbers
bool flag = true;                 // true or false
char letter = 'A';                // Single character
short mediumNumber = 32767;       // 16-bit integer
long bigNumber = 123456789L;      // Long integer
float floatNumber = 3.14f;        // Single precision

// Type Modifiers
unsigned int positive = 100;      // Only positive values
signed int withSign = -100;       // Positive and negative
long long veryBig = 1234567890LL; // Extra large numbers

// Constants
const double PI = 3.14159;        // Constant value
const char* NAME = "C++";         // Constant string

// Auto Type Deduction
auto autoInt = 42;                // Type deduced to int
auto autoDouble = 3.14;           // Type deduced to double

// References and Pointers
int& ref = number;                // Reference
int* ptr = &number;               // Pointer
```

## Operators
```cpp
// Arithmetic Operators
int sum = 5 + 3;                 // Addition
int difference = 10 - 4;         // Subtraction
int product = 4 * 2;             // Multiplication
int quotient = 15 / 3;           // Division
int remainder = 7 % 3;           // Modulus

// Compound Assignment
int number = 10;
number += 5;                     // Add and assign
number -= 3;                     // Subtract and assign
number *= 2;                     // Multiply and assign
number /= 4;                     // Divide and assign

// Comparison Operators
bool isEqual = (5 == 5);         // Equal to
bool notEqual = (5 != 3);        // Not equal to
bool greater = (7 > 3);          // Greater than
bool less = (4 < 8);             // Less than
bool greaterEqual = (5 >= 5);    // Greater than or equal
bool lessEqual = (4 <= 4);       // Less than or equal

// Logical Operators
bool and_result = true && false; // Logical AND
bool or_result = true || false;  // Logical OR
bool not_result = !true;         // Logical NOT

// Bitwise Operators
int bitAnd = 5 & 3;             // Bitwise AND
int bitOr = 5 | 3;              // Bitwise OR
int bitXor = 5 ^ 3;             // Bitwise XOR
int leftShift = 5 << 1;         // Left shift
int rightShift = 5 >> 1;        // Right shift

// Increment/Decrement
int count = 0;
count++;                         // Post-increment
++count;                         // Pre-increment
count--;                         // Post-decrement
--count;                         // Pre-decrement
```

## Control Structures
```cpp
// If-Else Statements
if (score >= 90) {
    cout << "A grade";
} else if (score >= 80) {
    cout << "B grade";
} else {
    cout << "Below B";
}

// Switch Statement
switch (day) {
    case 1:                      // Case with break
        cout << "Monday";
        break;
    case 2:                      // Fall-through case
        cout << "Tuesday";
        [[fallthrough]];         // C++17 attribute
    default:
        cout << "Other day";
}

// While Loop
int i = 0;
while (i < 5) {                 // Loop with condition
    cout << i << endl;
    i++;
}

// Do-While Loop
do {                           // Executes at least once
    cout << i << endl;
    i--;
} while (i > 0);

// For Loop
for (int j = 0; j < 5; j++) {  // Standard for loop
    cout << j << endl;
}

// Range-based For Loop
int numbers[] = {1, 2, 3, 4, 5};
for (int num : numbers) {       // Range-based loop
    cout << num << endl;
}
```

## Arrays
```cpp
// Array Declaration and Initialization
int numbers[5];                 // Fixed-size array
int values[] = {1, 2, 3, 4, 5}; // Array with initializer
const char* names[3];           // Array of strings

// Multi-dimensional Arrays
int matrix[3][3];              // 2D array
int grid[][3] = {              // 2D array with initializer
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Array Operations
numbers[0] = 10;               // Setting element
int value = numbers[0];        // Getting element
size_t length = sizeof(numbers) / sizeof(numbers[0]); // Array length

// Modern Array Alternatives
array<int, 5> modern_array = {1, 2, 3, 4, 5}; // std::array
vector<int> dynamic_array = {1, 2, 3};        // std::vector
```

## Functions
```cpp
// Basic Function
void printMessage() {
    cout << "Hello!" << endl;
}

// Function with Parameters
int add(int a, int b) {
    return a + b;
}

// Function with Multiple Returns
string getGrade(int score) {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    return "C";
}

// Function Overloading
void display(int number) {
    cout << "Number: " << number << endl;
}

void display(string text) {
    cout << "Text: " << text << endl;
}

// Default Parameters
void greet(string name = "World") {
    cout << "Hello, " << name << "!" << endl;
}

// Inline Function
inline int square(int x) {
    return x * x;
}
```

## String Operations
```cpp
// String Creation
string str1 = "Hello";         // std::string
const char* str2 = "Hi";       // C-style string

// String Methods
string text = "Hello World";
size_t length = text.length(); // String length
char first = text[0];          // Get character
string sub = text.substr(0, 5); // Substring
// No direct case conversion methods in std::string

// String Comparison
bool equals = (str1 == "Hello");     // Content comparison
int compare = str1.compare(str2);    // Compare strings

// String Manipulation
string concat = str1 + " " + str2;   // Concatenation
text.replace(0, 5, "Hi");           // Replace
text.append(" There");              // Append
text.insert(5, " ");               // Insert

// String Stream
stringstream ss;
ss << "Number: " << 42;            // Stream operations
string result = ss.str();          // Get string result
```
