# LRU cache implementation

Initialized with the max size of cache. Following implementation is not the fastest implementation that you can find online. It beats on 16 percent of C++ implementations on leetcode
Leetcode submission : https://leetcode.com/submissions/detail/500125587/

Data structures used are 
1. std::list
   
   Functions Used :

   1. erase(iteratorLocation) : erase element at passed iterator location
   2. emplace_front(value) : insert a new node in list with provided value
   3. back() : access the value associated with the last element of the list
   4. pop_back() : to pop the last element from list
   5. begin() : to find the begin iterator

2. unordered_map

    Functions Used :

    1. erase(iteratorLocation) : erase the element pointed to by the iterator location.
    2. find(element) : return the iterator location of passed element in map, if element not found then return the iterator pointing to the last element.
    3. end() : return the iterator location of one past the last element
```cpp
struct valueIterator {
    int value;
    std::list<int>::iterator listIterator;
};

class LRUCache {
    std::list<int> _lruCacheList; // list of keys
    std::unordered_map<int, valueIterator> _elementMap; // key : (value, addressInList)
    int _capacity;
    
    // move the element from current location in _lruCacheList to the front
    // Input is always valid a valid iterator location other than end()
    void reshuffleAndUpdateCache(std::unordered_map<int, valueIterator>::iterator mapIterator, int value)
    {
        auto lruCacheIterator = (*mapIterator).second.listIterator;
        _lruCacheList.erase(lruCacheIterator);
        _lruCacheList.emplace_front((*mapIterator).first);
        
        (*mapIterator).second.listIterator = _lruCacheList.begin();
        (*mapIterator).second.value = value;
    }
    
    // Delete last element from cache
    void deleteLastElementFromCache() {
        auto keyToDelete = _lruCacheList.back();
        _lruCacheList.pop_back();
        _elementMap.erase(_elementMap.find(keyToDelete));
    }
    
    void insertNewElementInCache(int key, int value) {
        _lruCacheList.emplace_front(key);
        _elementMap[key] = valueIterator{value, _lruCacheList.begin()};
    }
    
public:
    LRUCache(int capacity) :
    _capacity(capacity) {
    }
    
    int get(int key) {

        auto mapIter = _elementMap.find(key);
        if(mapIter != _elementMap.end()) {
            reshuffleAndUpdateCache(mapIter, (*mapIter).second.value);
            return (*mapIter).second.value;
        }
        return -1;
    }
    
    void put(int key, int value) {
        auto mapIter = _elementMap.find(key);
        if(mapIter != _elementMap.end()) // element already exist in Map
        {
            // Reshuffle the elements and update value
            reshuffleAndUpdateCache(mapIter, value);
        }
        else
        {
            // Element don't exist in the cache, insert the element
            if(_lruCacheList.size() == _capacity)
            {
                deleteLastElementFromCache();
            }
            insertNewElementInCache(key, value);
        }
    }
};

```

[HOME](../README.md)
