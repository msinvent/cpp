# Notes from CppCon 2015: Bjarne Stroustrup “Writing Good C++14" and Herb Sutter "Writing Good C++14... By Default”

A smaller, simpler C++

1. Coding guidelines
   1. Supported by "guidelines support library" (GSL)
   2. Supported by analysis tools.
   3. Core guidelines(available now)

Coding rules :

1. Are outdated
2. Are specialized
3. Are not understood by their users.
4. Are not well supported by tools
   1. Platform dependencies
   2. Compiler dependencies
   3. Expensive
5. Do not provide guidance

Rules tell people about what not to do, instead of telling what to do. Never tell people what to do without giving a reason.

Eliminate whole classes or errors :

1. Eliminate resource leaks
   1. Without loss of performance
2. Eliminate dangling pointers
   1. Without loss of performance
3. Eliminate out-of-range access
   1. With minimal cost

## Core Rules

### The core of core

1. No leaks
2. No dangling pointers
3. No type violations through pointers

How ?

1. Root every object in a scope
   1. vector<**T**>
   2. string
   3. ifstream
   4. unique_ptr<**T**>
   5. shared_ptr<**T**>
2. RAII
   1. No naked new
   2. No naked delete

## Herb

Some nomenclature that Herb uses SA(Stack Analysis)

### Initial target: Type and memory safety

Traditional definition = type-safe + bounds-safe + lifetime-safe

1. Type : Avoid unions, use variant
2. Bounds : Avoid pointer arithmetic, use array_view
3. Lifetime : Don't leak(forget to delete), don't currupt(double-delete), don't dangle(e.g. return &local)

Future += Concurrency, security

Herb mentioned about following example where rA is invalid for usage because the scope of returned variable is only extended if the returned type and local const reference have the same type.

```cpp
unique_ptr<A> myFun()
{
    return make_unique<A>();
}
void Example1() {
    // The scope of returned rvalue will not be extended 
    // by constant ref, because they both are of different type
    const A& rA = myFun(); 
    use(rA);
}
void Example2() {
    // The scope of returned rvalue will not be extended 
    // by constant ref, because they both are of different type
    auto p = myFun();
    const A& rA = *p; 
    use(rA);
}

```

[HOME](./../README.md)
