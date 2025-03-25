## Cycle in a Directed Graph

Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, check whether it contains any cycle or not.  
The graph is represented as an adjacency list, where adj[i] contains a list of vertices that are directly reachable from vertex i.   
Specifically, adj[i][j] represents an edge from vertex i to vertex j.


Constraints:  
1 ≤ V, E ≤ 10  

## Approach - 
use two array-
1.  vis[] - which track which node has been visited to avoid repeatation
2.  path[] - which kepp track of current path so that we could know if current node has been encounterd before in the same path - which mean cycle has found

use DFS to find cycle from each unvisited node
```

class Solution {
    // Function to detect cycle in a directed graph.
    public boolean isCyclic(ArrayList<ArrayList<Integer>> adj) {
        
        int size=adj.size();
        boolean vis[]=new boolean[size];
        boolean curPath[]=new boolean[size];
        for(int i=0; i<adj.size(); i++){
            if(!vis[i]){
                if(dfs(adj,vis,curPath,i)){
                    return true;
                }
            }
        }
        return false;
    }
    private boolean dfs(ArrayList<ArrayList<Integer>> adj, boolean vis[], boolean curPath[], int node){
        if(vis[node] && curPath[node]){
            return true;
        }
        if(vis[node]){
            return false;
        }
        vis[node]=true; curPath[node]=true;
        for(int adjNode : adj.get(node)){
            if(dfs(adj,vis,curPath,adjNode)){
                return true;
            }
        }
        curPath[node]=false;
        return false;
    }
}
