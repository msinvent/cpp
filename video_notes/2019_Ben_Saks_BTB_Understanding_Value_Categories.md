# Notes from CppCon 2019: Ben Saks “Back to Basics: Understanding Value Categories”

Conceptually, rvalues(of non-class type) don't occupy data storage in the object program. The tructh, some might. But, the programmer need to program assuming it will not.

Well, conceptually, lvalues occupy data storage. In truth, the optimizer might eliminate some of them(but, only when you won't notice it). C and C++ let you assume that lvalues always do occumy storage in memory.

## Rvalues of Class Type

Class type involves class, struct and enum. They occupy data storage.

Why is the difference ?

```cpp
int x = 1;
int y = 3;
x + 1 = y; // x + 1 is a rvalue thus this expression is invalid
```

But, for class variable access compiler uses a base offset wrt to the start location of object on data storage.

```cpp
struct foo{
    int x;
    int y;
}
int j = foo().y; // accessing y member of rvalue,
```

In above example, compiler uses a base+offset calculation to access foo().y. Therefore, the return value of foo() must have a base address. This is why rvalues of class types must be treated differently.

In fact, not all lvalues can appear on the left of an assignment. Example Non-modifiable Lvalues. An lvalue is non-modifiable if it has a const-qualifier type.

Other example,
Unscoped enumeration values implicitly convert to integer

```cpp
enum {
    MAX = 100;
}

MAX += 3; // error: MAX is an rvalue
int *p = &MAX; // error, MAX is an rvalue

// but, if we may MAX a constant integer

int const MAX2 = 100;
// now, when MAX2 appears in an expressino, it's a non-modifiable lvalue

MAX2 += 3; // error: MAX is non-modifiable
int const *p = &MAX2; // OK: MAX is an lvalue
```

## So far

|          | can take the address of | can assign to |
-----------| ------------------------| --------------
lvalue|yes|yes
non-modifiable lvalue|yes|no
(non-class)rvalue|no|no

## Reference types

- A referemce is essentially a pointer that's automatically dereferenced each time it's used.
- You can rewrite most, if not all, code that uses a const pointer.
- Reference can provide friendler function interfaces
- Most specifically, C++ has references so that overloaded operators can look just like build-in operators.
- **In C++ you can't overload an operator with a parameter of pointer type**

```cpp
class enum{
    Jan,
    Feb,
    March,
    April,
    May,
    June,
    July,
    August,
    September,
    October,
    November,
    December
};
month& operator++ (month &x) {
    return static_cat<month>((x+1)%12);
}
```

## "Reference to Const" Parameters

- A "reference to const" parameter will accept an argument that's either const or non-const.

```cpp
R f(T const& t);
```

- In constrast, a refence() to non-const) parameter will accept only a non-const argument.
- A reference to const yields a non-modifiable lvalue. Weather the passed argument is const or non-const, lvalue or rvalue, or any combination of both.

## References and Temporaries

A "pointer to T" can point only to an lvalue of type T. Similarly, a "reference to T" binds only to an lvalue of type T.

```cpp
int i;
double *pd = &i // can't convert pointers
double &rd = i; // can't bind this either
```

Exception to above rule, is that a reference to a const T can bind to an expressino x that't not an lvalue of type T.

```cpp
double const& rd = 3;
```

When program reaches this declaration

- program converts the value of 3 from int to double.
- Creates a temporary double to hold the converted result, and
- binds rd to the temporary.

When program leaves the scope containing rd, the program

- destroys the temporary

## Two kinds of Rvalues

```cpp
long double x;
void f(long double const& ld);

f(x); //passes a reference to x
f(1); //passes a reference to a temporary, containing 1 converted to long double
```

- Conceptually, rvalues of build-in types don't occupy data storage.
- However, the temporary object created because of reference binding have to. Even if it has a build-in type like long double.

- In modern C++

  1. "Pure rvalues" or prvalues, which don't occupy data storage.
  2. "Expiring values" or xvalues, which do.

- The temporary object is created through a **temporary materialization conversion**. It converts prvalue to an xvalue.

## References

1. C++11 introduced another kind of reference.
2. What C++03 calls "references", C++11 calls "lvalue references".
3. This distinguishes them from C++11's new "rvalue references".
4. lvalue reference in C++11 behave just like references in C++03.

### Rvalue references

1. Whereas an lvalue reference declaration uses & rvalue reference uses the && operator.
2. **rvalue reference can not bind to an lvalue reference. No matter what. Remember lvaue reference to const can bind to rvalue.**

    ```cpp
    int &&ri = 10;
    int n = 10;
    int &&ri = n; // error: n is an lvalue
    int const &&rj = n; // error: n is an lvalue
    ```

3. rvalue reference are mostly used a funciton parameters and return types. **Main objective to introduce them was to enable move operations**.
4. Binding an rvalue reference to an rvalue triggers a temporary materialization conversion. That changes a prvalue to xvalue. Just like binding reference to const to an rvalue.

## Move Operations

Modern C++ uses rvalue references to implement move operations that can avoid unnecessary copying. If we don't need the original object, or the original object in an rvalue then we may gain by passing the reference to that to the assigned to object.

```cpp
string s1, s2, s3;
s1 = s2; // Copy operation string::operator=(string const &)
s1 = s2 + s3; // string::operator=(string &&)
// The value expires at the end of the statement, so it can be safely moved
```

So, when we gind an **rvalue reference** to an rvalue then it creates an xvalue using *temporary materialization conversion**.

## Rvalue References as Lvalues

- Binding an "rvalue reference" to an rvalue creates an xvalue.
- However, look inside the move assignment operator:

```cpp
string &string::operator=(string&& other) {
    string temp(other); // calls string(string const &)
    // This will call copy constructor of string 
    // as other is an lvalue inside this function
}

template<typename T>
void swap(T& a, T& b) {
    T temp(a);  // copy construction
    a = b;      // copy assignment
    b = temp;   // copy assignment 
}
```

It's safe to move from an lvalue only if it's expiring. Like in the case of swap, we need not to perform deep construction(or, copy construction of temp using a). At times compiler can't easily descern this.

- How can we inform the compiler ?
  
  - Test 

If it has a name, its an lvalue. It it don't have a name then it may be an rvalue, because it may also be an lvalue.
[HOME](../README.md)
