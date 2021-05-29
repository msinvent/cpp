```cpp
// This implementation is approximately 420 times slower than 
// range_from + rand%(range_to-range_from), which may not generate truly uniform random numbers
// Reference to this code : https://stackoverflow.com/questions/7560114/random-number-c-in-some-range
template<typename T>
T random(T range_from, T range_to) {
    std::random_device                  rand_dev;
    std::mt19937                        generator(rand_dev());
    std::uniform_int_distribution<T>    distr(range_from, range_to);
    return distr(generator);
}


//////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////
// Bench Marking code for https://quick-bench.com/#
//////////////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////

#include <random>

template<typename T>
T random(T range_from, T range_to) {
    std::random_device                  rand_dev;
    std::mt19937                        generator(rand_dev());
    std::uniform_int_distribution<T>    distr(range_from, range_to);
    return distr(generator);
}

static void uniform_random(benchmark::State& state) {
  // Code inside this loop is measured repeatedly
  for (auto _ : state) {
    int i = random<int>(1000, 2000);
    benchmark::DoNotOptimize(i);
  }
}
// Register the function as a benchmark
BENCHMARK(uniform_random);

static void modulo_random(benchmark::State& state) {
  // Code before the loop is not measured
  for (auto _ : state) {
    int i = 1000 + std::rand()%(2000 - 1000);
    benchmark::DoNotOptimize(i);
  }
}
BENCHMARK(modulo_random);
```

[HOME](../README.md)
