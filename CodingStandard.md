<div align="center">
  <picture>
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/mozahzah/IECore/raw/master/Resources/IE-Brand-Kit/IE-Logo-Alt-NoBg.png?">
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/mozahzah/IECore/raw/master/Resources/IE-Brand-Kit/IE-Logo-NoBg.png?">
  <img alt="IELogo" width="128">
  </picture>
</div>

<h1 align="center">IE's C++ Coding Standards</h1>

## Naming Conventions

### Case Conventions
- **Types (classes, structs, enums)**: PascalCase
  - Examples: `IELog`, `IERenderer`
- **Functions**: PascalCase
  - Examples: `Initialize()`, `GetPlayerName()`
- **Variables**:
  - Public/Protected: PascalCase
  - Private members: PascalCase with `m_` prefix
  - Examples: `Count`, `m_CacheSize`

### File Naming Convention

The file name must match the name of the primary/most important class or struct contained within it. For example, if the file contains class `IEClass`, the files must be named `IEClass.h` and `IEClass.cpp`. Any auxiliary types defined in the same file (helper classes, enums, or support structures) should be considered secondary and not influence the file name. This naming convention ensures clear organization and makes it easy to locate the main class definition within your codebase. Even if a file contains multiple classes or types, only the most significant one should determine the file name.

Example:
```cpp
// IEClass.h

class IEHelperClass // Secondary class
{
    //...
}
enum class IEState // Secondary enum
{ 
    //...
}
class IEClass // Primary class - file is named after this
{
    //...
}
```

## Brace Placement

All opening braces must begin on a new line. This applies to all code blocks including functions, classes, control structures (if, while, for), and lambdas.

Examples:

```cpp
// Class definition
class IEClass
{
public:
    IEClass();
};

// Function definition
IEClass::Func(float Value)
{
    // ...
}

// If statement
if (IsValid())
{
    DoSomething();
}

// Lambda
auto lambda = [this]()
{
    DoWork();
};

// For loop
for (int32 i = 0; i < Count; ++i)
{
    Process(i);
}

// If-else chain
if (Condition1)
{
    HandleCase1();
}
else if (Condition2)
{
    HandleCase2();
}
else
{
    HandleDefault();
}
```

## Class/Struct Organization

### Header File (.h) Structure
Classes and structs should be organized in the following order:

1. **Nested Types**
    - Nested enums
    - Nested structs/classes
    - Type aliases (using/typedef)

2. **Special Member Functions**
    - Constructor(s)
    - Destructor
    - Copy/Move constructors
    - Assignment operators
    - Other operator overloads

3. **Public Interface**
    - Virtual Public member functions
    - Public member functions
    - Static public functions

4. **Protected Interface**
    - Virtual Protected member functions
    - Protected member functions
    - Static protected functions

5. **Private Interface**
    - Virtual Protected member functions
    - Private member functions
    - Static private functions

6. **Member Variables**
    - Static member variables
    - Public member variables
    - Protected member variables
    - Private member variables (with `m_` prefix)

#### Example Class Structure
```cpp
class IEClass
{
    // 1. Nested Types
public:
    using ValueType = T;
    struct IEClassNestedStruct
    {
        int x = 0;
        int y = 0;
    }

    // 2. Special Member Functions
public:
    IEClass() = default;
    IEClass(float Value) : 
        m_PrivateValue(Value) 
    {
        //...
    }
    ~IEClass();
    
    IEExampleClass(const IEClass& Other);
    IEClass& operator=(const IEClass& Other);
    
    bool operator==(const IEClass& Other) const;
    
    // 3. Public Interface
public:
    virtual void Initialize();
    virtual void Deinitialize();

public:
    void GetName();
    bool IsValid() const;
    
    // 4. Protected Interface
protected:
    virtual void UpdateState();
    virtual void OnInternalStateChange();

protected:
    void UpdateInternalState();
    void RequestExit();
    
    // 5. Private Interface
private:
    void CleanupResources();
    
    // 6a. Public Member Variables
public:
    FString Name;
    int32 PublicCounter;
    
    // 6b. Protected Member Variables
protected:
    float ProtectedValue;
    
    // 6c. Private Member Variables
private:
    static constexpr int Constant = 64;
    float m_PrivateValue;
    bool m_IsInitialized;
};
```
### Implementation File (.cpp) Organization

#### Function Definition Order
- Function definitions in the .cpp file must match the declaration order in the header file
- This maintains consistency and makes code navigation easier
- Includes special member functions, followed by public, protected, and private functions

#### Constructor Initializer Lists
Constructor initializer lists with multiple initializers must place each initializer on a separate line with a single level of indentation. The colon remains on the same line as the constructor declaration, and each initializer is indented once. This style improves readability when dealing with multiple member initializations.

Example:
```cpp
IEClass::IEClass(float Value, int32 Count) : 
    m_PrivateValue(Value),
    m_Count(Count),
    m_IsValid(true)
{
    Initialize();
}
```

### Additional Best Practices

- **Member Variable Initialization**
   - Initialize member variables in constructors when needed
   - Prefer in-class initializers for simple cases
   - Follow the same order as declaration

- **Const Correctness**
   - Mark member functions as const when they don't modify object state
   - Use const references for parameters when possible

- **Access Specifiers**
   - Use the most restrictive access level possible
   - Group members by access level
   - Clearly separate sections with access specifier keywords

## Cache Line Awareness and Memory Alignment

### Cache Line Organization
* Standard cache line size is 64 bytes on most modern CPUs
* Group frequently accessed members together to minimize cache misses
* Order member variables by size **(largest to smallest)** to minimize padding
* Consider using alignas() for critical structures

Example of poor vs. optimal cache alignment:
```cpp
// Poor cache alignment - causes unnecessary padding
class PoorAlignment
{
private:
    char m_Byte;           // 1 byte + 7 padding
    double m_Double;       // 8 bytes
    char m_AnotherByte;    // 1 byte + 7 padding
    int32 m_Integer;       // 4 bytes + 4 padding
};  // Total: 32 bytes (16 bytes wasted on padding)

// Optimal cache alignment
class GoodAlignment
{
private:
    double m_Double;       // 8 bytes
    int32 m_Integer;       // 4 bytes
    char m_Byte;          // 1 byte
    char m_AnotherByte;   // 1 byte
    // 2 bytes padding
};  // Total: 16 bytes (only 2 bytes of padding)
```