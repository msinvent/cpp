# Notes from CppCon 2019: Mathieu Ropert "This Videogame Programmer Used the STL and You Will Never Guess What Happened Next"

Probable Issues with STL :

1. STL familiarity
2. STL availability
3. STL bloat
   1. Vendor implementations may include additional debug features to help developers
   2. There is a build flag somewhere to turn them off.
   3. C++ design principles dictate that unused features should not be added to the cost.
   4. But, in practive at the least it will add to the compilation time.(if you are using SNINAE)

## Key principle followed in STL

    Separation of datastructure and algorithm 

## The Quest for Performance

1. Games need to run within a timebox
2. Worst case scenarios and unpredictable latency matter a lot.
3. Common wisdom recommends low level languages for better control over performance.
4. STL comes with some degree of abstraction
   1. Templates
   2. Iterators
   3. Debug/checked iterators
   4. Proxy iterators
5. Required a good optimizer to yield performance.

## Performance in 2019

1. The 90486 was the last x86 to run instructions sequentially.
2. Modern CPUs execute instructions out of order.
3. How does "low level" imperative C fare without optimization today ?
4. C++ abstractions will be slower than raw C with all optimizations turned off.
5. Both C and C++ are an order of magnitude slower when you disable optimizations.
6. Enabling even minimal optimizatinos yields enormous gains.

## Containers overview

### std::vector

Greatness is :

1. Heap-allocated array that can be resized.
2. Cheap to move and random access.
3. Its as fast as it can get to iterate over
   1. Cost ore reaching L1 (3-4 CPU cycles), L2(10-12 CPU cycles), L3(30-70 CPU cycles), RAM(100-150 CPU cycles)
   2. Vector keep most of it in cache line and cache as elements are adjacent in memory.
4. Modern CPU caching can have 1-100 impact on performance.
5. O(n) operations on std::vector can outperform O(log n) on other containers.

Limitations are :

1. Growth factor is neigher specified nor configurable(most commonly 1.5 or 2)
2. Standard specifications prohibits small buffer optimization
3. std::vector<**bool**> is a mess.

Alternatives :

1. boost::small_vector
2. Avoid heap allocation for small sizes
3. May be O(n) on move(and invalidate iterators).

### std::array

1. Stack-allocated array with fixed size.
2. O(1) random acess and cache friendly layout.
3. O(n) to move, potentially as expensive as copy.
4. Major limitation is Fixed size, not capacity.
5. Not good for dynamic insertion.

Alternatives :

1. boost::static_vector

Rest of the talk talked about how std::map is not good and std::map is a better alternative for speed. But, faster implementations of HASH map are available at the cost of memory. Ropert recommended pushing the compiler vendor to better quality and reporting bugs or fixing them if we can.

[HOME](../README.md)
