```cpp
// Sample usage of partial_sum
// Reference of this code : https://en.cppreference.com/w/cpp/algorithm/partial_sum
// partial_sum is defined in the numeric header
// The output vector need to have same size if you wan to use it

#include <numeric>
#include <vector>
#include <iostream>
 
int main()
{
    std::vector<int> w{2, 2, 2, 5, 2, 2, 3};
    std::vector<int> prefix_sum;
    prefix_sum.resize(w.size());
    std::partial_sum(w.begin(), w.end(), prefix_sum.begin());
    std::cout<<"Prefix Sum : ";
    for(auto i : prefix_sum)
    {
        std::cout<<i<<" ";
    }std::cout<<"\n";

    std::vector<int> prefix_product(w.size(), 0);
    std::partial_sum(w.begin(), w.end(), prefix_product.begin(), std::multiplies<int>());
    std::cout<<"Prefix Product : ";
    for(auto i : prefix_product)
    {
        std::cout<<i<<" ";
    }std::cout<<"\n";

    return 0;
}
```

[HOME](../README.md)
