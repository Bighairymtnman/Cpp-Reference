# C++ Advanced Concepts

## Table of Contents
- [Templates](#templates)
- [Smart Pointers](#smart-pointers)
- [STL Containers](#stl-containers)
- [Lambda Expressions](#lambda-expressions)
- [Move Semantics](#move-semantics)
- [Memory Management](#memory-management)

## Templates
```cpp
// Function Template
template<typename T>
T max(T a, T b) {                // Generic function
    return (a > b) ? a : b;
}

// Class Template
template<typename T, typename U>
class Pair {
private:
    T first;
    U second;

public:
    Pair(T f, U s) : first(f), second(s) {}
    
    // Member function template
    template<typename V>
    V convert() const {
        return static_cast<V>(first + second);
    }
};

// Template Specialization
template<>
class Pair<bool, bool> {         // Specialized for booleans
    // Special implementation
};

// Variadic Templates
template<typename T>
T sum(T t) {                     // Base case
    return t;
}

template<typename T, typename... Args>
T sum(T first, Args... args) {   // Recursive case
    return first + sum(args...);
}

// SFINAE (Substitution Failure Is Not An Error)
template<typename T>
typename std::enable_if<std::is_integral<T>::value, bool>::type
    isEven(T t) {
    return t % 2 == 0;
}
```

## Smart Pointers
```cpp
// Unique Pointer
unique_ptr<int> ptr1(new int(42));     // Exclusive ownership
auto ptr2 = make_unique<int>(42);      // Preferred way

// Moving unique_ptr
unique_ptr<int> ptr3 = move(ptr1);     // Transfer ownership

// Shared Pointer
shared_ptr<string> ptr4 = make_shared<string>("Hello");
shared_ptr<string> ptr5 = ptr4;        // Reference counting
cout << ptr4.use_count();              // Count references

// Weak Pointer
weak_ptr<string> weak = ptr4;          // No ownership
if (auto shared = weak.lock()) {       // Check if object exists
    cout << *shared;
}

// Custom Deleter
auto deleter = [](int* p) {
    cout << "Deleting " << *p << endl;
    delete p;
};
unique_ptr<int, decltype(deleter)> ptr6(new int(100), deleter);
```

## STL Containers
```cpp
// Vector Operations
vector<int> vec = {1, 2, 3};
vec.push_back(4);                      // Add element
vec.emplace_back(5);                   // Construct in place
vec.reserve(10);                       // Reserve capacity

// List Operations
list<string> lst = {"one", "two"};
lst.push_front("zero");                // Add at front
lst.splice(lst.begin(), lst);          // Splice elements

// Map Operations
map<string, int> scores;
scores.insert({"Alice", 100});         // Insert pair
scores.emplace("Bob", 95);             // Construct in place
auto [iter, success] = scores.try_emplace("Charlie", 90);

// Unordered Containers
unordered_map<string, int> hash_map;   // Hash table
unordered_set<int> hash_set;           // Hash set

// Container Adaptors
stack<int> stk;                        // LIFO
queue<int> que;                        // FIFO
priority_queue<int> pq;                // Heap

// Custom Comparator
auto comp = [](const string& a, const string& b) {
    return a.length() < b.length();
};
set<string, decltype(comp)> ordered(comp);
```

## Lambda Expressions
```cpp
// Basic Lambda
auto add = [](int a, int b) { return a + b; };

// Lambda with Capture
int multiplier = 10;
auto multiply = [multiplier](int x) { return x * multiplier; };

// Mutable Lambda
auto counter = [count = 0]() mutable {
    return ++count;
};

// Generic Lambda (C++14)
auto generic = [](auto x, auto y) {
    return x + y;
};

// Lambda with Capture by Reference
int value = 42;
auto modifier = [&value]() { value *= 2; };

// Complex Lambda with Multiple Captures
auto processor = [&value, multiplier](int input) {
    value = input * multiplier;
    return value;
};
```

## Move Semantics
```cpp
// Move Constructor
class Buffer {
private:
    int* data;
    size_t size;

public:
    // Constructor
    Buffer(size_t s) : size(s) {
        data = new int[size];
    }

    // Move Constructor
    Buffer(Buffer&& other) noexcept 
        : data(other.data), size(other.size) {
        other.data = nullptr;           // Transfer ownership
        other.size = 0;
    }

    // Move Assignment
    Buffer& operator=(Buffer&& other) noexcept {
        if (this != &other) {
            delete[] data;              // Clean up existing
            data = other.data;          // Transfer ownership
            size = other.size;
            other.data = nullptr;
            other.size = 0;
        }
        return *this;
    }

    // Destructor
    ~Buffer() {
        delete[] data;
    }
};

// Using Move Semantics
Buffer createBuffer(size_t size) {
    return Buffer(size);                // Move construction
}

Buffer buf = createBuffer(1000);        // Move assignment
```

## Memory Management
```cpp
// Raw Memory Operations
void* raw = operator new(sizeof(int));  // Raw allocation
operator delete(raw);                   // Raw deallocation

// Placement New
char buffer[sizeof(string)];
string* str = new (buffer) string("Hello"); // Construct in buffer
str->~string();                         // Manual destruction

// Custom Allocator
template<typename T>
class CustomAllocator {
public:
    T* allocate(size_t n) {
        return static_cast<T*>(::operator new(n * sizeof(T)));
    }

    void deallocate(T* p, size_t n) {
        ::operator delete(p);
    }
};

// Memory Pool
class MemoryPool {
private:
    struct Block {
        Block* next;
    };
    Block* head = nullptr;

public:
    void* allocate(size_t size) {
        if (head == nullptr) {
            // Allocate new block
            return ::operator new(size);
        }
        Block* block = head;
        head = head->next;
        return block;
    }

    void deallocate(void* p) {
        Block* block = static_cast<Block*>(p);
        block->next = head;
        head = block;
    }
};

// RAII Pattern
class ResourceGuard {
private:
    Resource* resource;

public:
    ResourceGuard(Resource* r) : resource(r) {}
    ~ResourceGuard() {
        delete resource;                // Automatic cleanup
    }
};
```
