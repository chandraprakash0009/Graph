## Undirected Graph Cycle
Given an undirected graph with V vertices labelled from 0 to V-1 and E edges, check whether the graph contains any cycle or not.   
The Graph is represented as an adjacency list, where adj[i] contains all the vertices that are directly connected to vertex i.  

NOTE: The adjacency list represents undirected edges, meaning that if there is an edge between vertex i and vertex j, both j will be adj[i] and i will be in adj[j].

Examples:

Input: adj = [[1], [0,2,4], [1,3], [2,4], [1,3]] 
Output: 1
Explanation: 
1->2->3->4->1 is a cycle.  

Input: adj = [[], [2], [1,3], [2]]
Output: 0
Explanation:  No cycle in the graph.  
Constraints:  
1 ≤ adj.size() ≤ 105  

## Using DFS - 
Suppose we are at a node X, there is a adjacent node Y of X   
1. if node Y is not visited then visit to that node and keep track of node from where we have came from.
2. if node Y has been already visited and currently we have came at X from a node other than node Y(to avoid to going to the same node from which currently we have came from ), it means that cycle is formed
```class Solution {
    // Function to detect cycle in an undirected graph.
    
    public boolean isCycle(ArrayList<ArrayList<Integer>> adj) {
        // Code here
        boolean vis[]=new boolean[adj.size()];
        for(int i=0; i<adj.size(); i++){
            if(!vis[i]){
                if(rec(i,-1, adj,vis)){
                    return true;
                }
            }
        }
        return false;
    }
    private boolean rec(int node, int parent, ArrayList<ArrayList<Integer>> adj, boolean vis[]){
        vis[node]=true;
        for(int adjNode : adj.get(node)){
            if(!vis[adjNode]){
                if(rec(adjNode,node,adj,vis)){
                    return true;
                }
            }else if(adjNode!=parent){
                return true;
            }
            
        }
        return false;
    }
}
```
## Approach 2 - BFS
using bfs, find a node which is already visited from any other node than current node
``` 

class Solution {
    // Function to detect cycle in an undirected graph.
    class Pair{
        int num, parent;
        Pair(int n, int p){ num=n; parent=p; }
    }
    public boolean isCycle(ArrayList<ArrayList<Integer>> adj) {
        // Code here
        boolean vis[]=new boolean[adj.size()];
        Arrays.fill(vis,false);
        for(int i=0; i<vis.length; i++){
            if(!vis[i]){
                vis[i]=true;
                if(containCycle(adj,i,vis)){ return true;}
            }
        }
        return false;
    }
    private boolean containCycle(ArrayList<ArrayList<Integer>> adj, int src, boolean vis[]){
        Queue<Pair> q =new LinkedList<>();
        q.add(new Pair(src,-1));
        while(!q.isEmpty()){
            Pair  item = q.remove();
            int node =item.num;
            for(int adjNode : adj.get(node)){
                if(!vis[adjNode]){
                    vis[adjNode]=true;
                    q.add(new Pair(adjNode,node));
                }else if(item.parent!=adjNode){
                    return true;
                }
            }
        }
        return false;
    }
}
