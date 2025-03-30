# C++ Basics Quick Reference

## Table of Contents
- [Classes and Objects](#classes-and-objects)
- [Inheritance](#inheritance)
- [Virtual Functions](#virtual-functions)
- [Abstract Classes](#abstract-classes)
- [Polymorphism](#polymorphism)
- [Encapsulation](#encapsulation)

## Classes and Objects
```cpp
// Basic Class Structure
class Car {
private:
    string brand;            // Private member variables
    int year;
    bool isRunning;

public:
    // Constructor
    Car(string b, int y) {
        brand = b;          // Initialize members
        year = y;
        isRunning = false;
    }

    // Constructor with initializer list (preferred)
    Car(string b, int y) : brand(b), year(y), isRunning(false) {
        // Body can be empty when using initializer list
    }

    // Member function (method)
    void startEngine() {
        isRunning = true;
        cout << "Engine started!" << endl;
    }

    // Getter method
    string getBrand() const {    // 'const' promises not to modify object
        return brand;
    }

    // Setter method
    void setYear(int y) {
        if (y > 1900) {         // Data validation
            year = y;
        }
    }

    // Destructor (called when object is destroyed)
    ~Car() {
        cout << "Car object destroyed" << endl;
    }
};

// Creating and Using Objects
Car myCar("Toyota", 2020);      // Create object
myCar.startEngine();            // Call method
string brand = myCar.getBrand(); // Use getter
```

## Inheritance
```cpp
// Base Class
class Animal {
protected:                      // Accessible in derived classes
    string name;

public:
    // Constructor
    Animal(string n) : name(n) {}

    // Virtual function (can be overridden)
    virtual void makeSound() {
        cout << "Some sound" << endl;
    }

    // Virtual destructor (important for proper cleanup)
    virtual ~Animal() {}
};

// Derived Class
class Dog : public Animal {     // Public inheritance
private:
    string breed;

public:
    // Constructor
    Dog(string n, string b) : Animal(n), breed(b) {}

    // Override base class method
    void makeSound() override { // 'override' keyword is good practice
        cout << "Woof!" << endl;
    }

    // Dog-specific method
    void fetch() {
        cout << name << " is fetching!" << endl;
    }
};

// Using Inheritance
Dog myDog("Rex", "Labrador");
myDog.makeSound();             // Calls Dog's version
myDog.fetch();                 // Dog-specific method
```

## Virtual Functions
```cpp
// Virtual Function Example
class Shape {
public:
    virtual double area() const {
        return 0.0;
    }

    // Pure virtual function (makes class abstract)
    virtual double perimeter() const = 0;

    // Virtual destructor
    virtual ~Shape() {}
};

class Circle : public Shape {
private:
    double radius;

public:
    Circle(double r) : radius(r) {}

    // Override virtual function
    double area() const override {
        return 3.14159 * radius * radius;
    }

    // Implement pure virtual function
    double perimeter() const override {
        return 2 * 3.14159 * radius;
    }
};

// Virtual Function Usage
Shape* shape = new Circle(5);   // Polymorphic behavior
double area = shape->area();    // Calls Circle's version
delete shape;                   // Proper cleanup
```

## Abstract Classes
```cpp
// Abstract Class (Interface)
class Drawable {
public:
    // Pure virtual functions
    virtual void draw() = 0;
    virtual void resize(int size) = 0;
    
    // Virtual destructor
    virtual ~Drawable() {}
};

// Concrete Class implementing abstract class
class Rectangle : public Drawable {
private:
    int width, height;

public:
    Rectangle(int w, int h) : width(w), height(h) {}

    // Must implement all pure virtual functions
    void draw() override {
        cout << "Drawing rectangle" << endl;
    }

    void resize(int size) override {
        width *= size;
        height *= size;
    }
};

// Multiple Inheritance (C++ specific)
class Square : public Shape, public Drawable {
    // Must implement all pure virtual functions from both bases
};
```

## Polymorphism
```cpp
// Polymorphic Class Hierarchy
class Vehicle {
public:
    virtual void move() {
        cout << "Vehicle moving" << endl;
    }
    virtual ~Vehicle() {}
};

class Car : public Vehicle {
public:
    void move() override {
        cout << "Car driving" << endl;
    }
};

class Boat : public Vehicle {
public:
    void move() override {
        cout << "Boat sailing" << endl;
    }
};

// Polymorphic Usage
void startJourney(Vehicle* vehicle) {
    vehicle->move();           // Calls appropriate version
}

int main() {
    Car car;
    Boat boat;
    
    startJourney(&car);       // "Car driving"
    startJourney(&boat);      // "Boat sailing"

    // Smart pointer usage (modern C++)
    unique_ptr<Vehicle> v1 = make_unique<Car>();
    v1->move();               // Safe polymorphic call
}
```

## Encapsulation
```cpp
// Fully Encapsulated Class
class BankAccount {
private:
    string accountNumber;
    double balance;
    string ownerName;

    // Private helper function
    void notifyOwner(const string& message) {
        cout << "Notification to " << ownerName << ": " << message << endl;
    }

public:
    // Constructor
    BankAccount(string accNum, string owner)
        : accountNumber(accNum), balance(0.0), ownerName(owner) {}

    // Const member function (doesn't modify object)
    double getBalance() const {
        return balance;
    }

    // Member functions with error handling
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            notifyOwner("Deposit successful");
        }
    }

    bool withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            notifyOwner("Withdrawal successful");
            return true;
        }
        return false;
    }

    // Copy constructor (explicit control of copying)
    BankAccount(const BankAccount& other)
        : accountNumber(other.accountNumber),
          balance(other.balance),
          ownerName(other.ownerName) {}

    // Assignment operator
    BankAccount& operator=(const BankAccount& other) {
        if (this != &other) {
            accountNumber = other.accountNumber;
            balance = other.balance;
            ownerName = other.ownerName;
        }
        return *this;
    }
};

// Using Encapsulated Class
BankAccount account("123456", "John Doe");
account.deposit(1000);        // Safe transaction
double balance = account.getBalance(); // Safe access
```
