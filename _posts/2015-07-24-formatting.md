---
layout: post
title: Naming & Formatting Guidelines
---

# Introduction 

This a note of the naming and formatting guidelines for c++, phyton and markdown language. These are very commonly used languages in research.

## C++
This is a note of [STAR C++ Naming & Formatting Guidelines](https://www.star.bnl.gov/public/comp/sofi/guidelines/formatguide.xml).

1.  Naming 

    1.  variables:
        *   camel case convention; 
        *   Should be nouns;
        *   No underscores are allowed;
        *   Starts with lowercase letter. 
        *   Class member variables are prefixed with `m`
        *   Global variables are prefixed with `g`
        *   Static data members are prefixed with `s`
        *   No `m` or `s` prefix for struct members

    2.  Functions:
        *   Starts with lowercase letter. The name of a function should be a verb.
            `addTableEntry();`
        *   boolean function should be prefixed with `is` or `has`
        *   Accessors match the name of the variable they are getting. DO NOT use `set` in ROOT
            `int numberOfEntries() const { return mNumberOfEntries; }`
        *   Mutators also match the name of the variable. Use the prefixes `set`
            `void setNumberOfEntries(int val) { mNumberOfEntries = val; }`

    3.  namespaces
        *   Starts with uppercase letter;

    4.  Type
        *   Starts with uppercase letter; `MyClass, MyEnum`
        *   Classes, structs, typedefs, and enums have same naming convention.

    5.  constant expressions
        *   Starts with uppercase letter; 
            `constexpr double LightSpeed = 2.99792458e+8;`

    6.  Enumeration and enumerator names

    ```c++
    int foo;
    ```

        *   Unscoped enumerator should be prefixed or postfixed with the enumeration's name

            ```c++
            enum Something 
            {
            SomethingSmall,   // the type name as prefix
            SomethingBig,
            SomethingElse
            };
            void add(Something);
            add(SomethingSmall);
            add(SomethingElse);
            ```

        *   Enum classes create scoped enumerators. Therefore there is no need for prefixing or postfixing.

            ```c++
            enum class Something : int {
            Small,
            Big,
            Unknown
            };
            void add(Something);
            add(Something::Small);
            add(Something::Unknown);
            ```

        
