# Is Graph Bipartite

[leetcode link](https://leetcode.com/problems/is-graph-bipartite/)

Example Inputs

Input:

1. graph = [[1,3],[0,2],[1,3],[0,2]]
2. graph = [[1,2,3],[0,2],[0,1,3],[0,2]]

## Constraints

1. graph.length == n
2. 1 <= n <= 100
3. 0 <= graph[u].length < n
4. 0 <= graph[u][i] <= n - 1
5. graph[u] does not contain u.
6. All the values of graph[u] are unique.
7. If graph[u] contains v, then graph[v] contains u.

```cpp
class Solution {
    
    bool traverserConnectedGraph(vector<vector<int>>& graph, int start, 
                                 std::vector<int>& alreadyTraversed) {
        
        std::queue<std::pair<int, int>> qu;        
        qu.push(std::pair<int, int>(start, -1));
        
        while(!qu.empty()) {
            auto quFront = qu.front();
            qu.pop();
            
            auto ptrInAlreadyTraversed = static_cast<bool>(alreadyTraversed[quFront.first]);
            if(ptrInAlreadyTraversed) {
                if(alreadyTraversed[quFront.first] != quFront.second) {
                    return false;
                }
            } else {
                alreadyTraversed[quFront.first] = quFront.second;
                for(auto& neighbor : graph[quFront.first]) {
                    qu.push(std::pair<int, int>(neighbor, (-1)*quFront.second));
                }
            }
        }
        return true;
    }
    
public:
    bool isBipartite(vector<vector<int>>& graph) {
        std::vector<int> alreadyTraversed(graph.size(), 0);
        
        while(true) {
            // find first non-traversed element
            auto nonTraversedNode = std::find(
                alreadyTraversed.begin(), alreadyTraversed.end(), 0);
            if( nonTraversedNode == alreadyTraversed.end()) {
                break;
            }
            if(!traverserConnectedGraph(graph, 
                                    nonTraversedNode - alreadyTraversed.begin(), 
                                    alreadyTraversed))
            {
                return false;
            }
        } 
        return true;
    }
};
```

Alternative solution

```cpp
class Solution {
    
public:
    bool isBipartite (vector<vector<int>>& graph) {
    int color[graph.size()];
    memset(color, 0, graph.size()*sizeof(color[0]));
    color[0] = 1;
    
    for(int i = 0; i < graph.size(); ++i){
        for(int j = 0 ; j < graph[i].size(); ++j){
            if(color[graph[i][j]] == 0){
                color[graph[i][j]] = ~color[i];
            }else if(color[graph[i][j]] == color[i]){
                return false;
            }
        }
    }
    
    return true;
    }
};
```

[HOME](../README.md)
