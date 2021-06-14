# Notes from CppCon 2019: Kate Gregory "Naming is Hard: Let's Do Better"

Kate gregory talked about this case when a company wanted find the top 10 contributers of revenue from a list.

We can perform

1. **sort** and then provide the top 10 element.
2. Or, rather than performing sort on the entire list we can perform **partial_sort** (Which only sorts the top 10 element and keep the rest of them in randomized order required by the internal algorithm)
3. Or, may be **partial_sort_copy** if the list is already sorted by some other index and then we want to keep the list in that order after answering our query.

I believe that a better algorithm to get this is **partition** using **nth_element**, as we only need to get the top contributers but gregory mentioned that we need to find the top 10 contributers, no where she mentioned that we need them in any order.

On performance comparison I reasized that std::partial_sort is doing better when the number of top elements that you want to look at is very small. Example in the following example the performance is almost equal for num = 400, for lower values of num std::partial_sort is better, for larger std::nth_element is better.

|n|partial_faster|nth_faster|
-|-|-
1|3.2|
400|benchmarking crashing|not-working
500||1.1
1000||3.1
2000||8.4
5000||23
5000||110

**I am still not sure if this is the right mechanism to compare speed, as the vector will be already partially-sorted or partitioned after the first iteration only.**

```cpp
#include <algorithm>

static void partialSort(benchmark::State& state) {
  // Code inside this loop is measured repeatedly
  constexpr std::size_t sz = 10000;
  constexpr uint32_t num = 100;
  std::vector<int> vec(sz, 0);
  srand(time(0));
  generate(vec.begin(), vec.end(), rand);
  for (auto _ : state) {
    std::partial_sort(vec.begin(), vec.begin() + num, vec.end());
    benchmark::DoNotOptimize(vec);
  }
}
// Register the function as a benchmark
BENCHMARK(partialSort);

static void nthElement(benchmark::State& state) {
  // Code before the loop is not measured
  constexpr std::size_t sz = 10000;
  constexpr uint32_t num = 100;
  std::vector<int> vec(sz, 0);
  srand(time(0));
  generate(vec.begin(), vec.end(), rand);
  for (auto _ : state) {
    std::nth_element(vec.begin(), vec.begin() + num, vec.end());
    benchmark::DoNotOptimize(vec);
  }
}
BENCHMARK(nthElement);

```
[HOME](../README.md)