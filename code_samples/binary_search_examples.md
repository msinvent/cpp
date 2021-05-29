# Binary Search and associated functions

First thing first the prerequisite is that the input is sorted. In some cases the situation can be a little relaxed(partitioned input only, but, for sake of simplicity lets alwasys feed sorted inputs). All of these are present in **algorithm** header file.

## std::binary_search (don't provide location)

### Complexity

On average, logarithmic in the distance between first and last: Performs approximately log2(N)+2 element comparisons (where N is this distance).
On **non-random-access iterators, the iterator advances produce themselves an additional linear complexity in N on average**.

### Returns

true if an element equivalent to val is found, and false otherwise.

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

int main() {
    std::vector<int> v{1,2,3,4,5,4,3,2,1};
    if(std::binary_search (v.begin(), v.end(), 3))
    {
        std::cout<<"3 found\n";
    }
}
```

## If you need location use lower_bound, upper_bound or equal range

### std::lower_bound (first not less)

Returns an iterator pointing to the first element in the range [first,last) which does not compare less than val. So the first element that is either equal or greater will be pointed by return iterator. Or else the end if none such element found.

### std::upper_bound (first greater)

Returns an iterator pointing to the first element in the range [first,last) which compares greater than val. So the first element that is either equal or greater will be pointed by return iterator. Or else the end if none such element found.

### std::equal_range (If we want both)

Returns the bounds of the subrange that includes all the elements of the range [first,last) with values equivalent to val.

The values are the same as those that would be returned by functions lower_bound and upper_bound respectively. Returns a pair of iterators.

```cpp
    bounds=std::equal_range (v.begin(), v.end(), 20);   
    30 30 20 20 20 10 10 10
    _____ ^_______ ^_______
```

**Some detail to keep in mind. If val is not equivalent to any value in the range, the subrange returned has a length of zero, with both iterators pointing to the nearest value greater than val, if any, or to last, if val compares greater than all the elements in the range.**

[HOME](../README.md)
