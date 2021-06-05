# Deep copy a graph

Multiple implementations are possible, stack space complexity depends on the search style (DFS, BFS)

Time Complexity = O(m + n), where m in the number of edges, and n is number of nodes
Space Complexity = O(n)

## Depth first search

Space occupied by recursion stack = O(H), height of the graph

## Bredth first search

Space occupied by recursion stack = O(W), width of the graph

### On a side note

If you want to only find if an elemen is present of not in a map then count method can be used. This will simplify the syntax from

```cpp
if(mymap.find(element) != mymap.end()) {

}
```

to

```cpp
if(mymap.count()) {

}
```

Anyways the solution written by me can be found here [Leetcode Link](https://leetcode.com/submissions/detail/502964433/)

I maintained a separate map for maintaining alreadyCreatedNodes. As I wanted to create nodes at the time of putting the neighbors in the queue. I like this design better as we need not to keep track of parent in this case.

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution {
public:
    Node* cloneGraph(Node* node) {
        if ( node == nullptr ) {
            return nullptr;
        }
        
        // Map of val to new Node* in copied graph, 
        // can be used as this number is unique
        unordered_map<Node*, Node*> alreadyCreatedNodes;
        unordered_set<Node*> alreadyVisited;
        std::queue<Node*> nodeQueue;
        
        Node* baseNode = new Node(node->val);
        nodeQueue.push(node);
        alreadyCreatedNodes[node] = baseNode;
        
        // We will traverse on the original graph and create another graph
        while(!nodeQueue.empty()) {
            auto nodePtr = nodeQueue.front();
            nodeQueue.pop();
            
            // If already visited skip
            if(alreadyVisited.find(nodePtr) != alreadyVisited.end())
            {
                continue;
            }

            alreadyVisited.insert(nodePtr);
            auto newGraphNodePtr = alreadyCreatedNodes[nodePtr];
            
            for(auto& neighbor : nodePtr->neighbors){
                if(alreadyVisited.find(neighbor) == alreadyVisited.end()) {
                    nodeQueue.push(neighbor);
                }
                
                // Loop over neighbors to initialize newGraphNodePtr->neighbors
                auto neighborAt = alreadyCreatedNodes.find(neighbor);
                if( neighborAt != alreadyCreatedNodes.end())
                {
                    // neighbor already created, just link
                    newGraphNodePtr->neighbors.emplace_back((*neighborAt).second);
                } else {
                    auto newNode = new Node(neighbor->val);
                    alreadyCreatedNodes[neighbor] = newNode;
                    newGraphNodePtr->neighbors.emplace_back(newNode);
                }
            }
        }
        
        return baseNode;
    }
};
```

[HOME](../README.md)
