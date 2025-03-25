## 785. Is Graph Bipartite?

There is an undirected graph with n nodes, where each node is numbered between 0 and n - 1. You are given a 2D array graph, where graph[u] is an array of nodes that node u is adjacent to. More formally, for each v in graph[u], there is an undirected edge between node u and node v.   
The graph has the following properties:

1.  There are no self-edges (graph[u] does not contain u).  
2.  There are no parallel edges (graph[u] does not contain duplicate values).  
3.  If v is in graph[u], then u is in graph[v] (the graph is undirected).  
4.   The graph may not be connected, meaning there may be two nodes u and v such that there is no path between them.  
A graph is bipartite if the nodes can be partitioned into two independent sets A and B such that every edge in the graph connects a node in set A and a node in set B.  

Return true if and only if it is bipartite.  

 

Example 1:


Input: graph = [[1,2,3],[0,2],[0,1,3],[0,2]]  
Output: false  
Explanation: There is no way to partition the nodes into two independent sets such that every edge connects a node in one and a node in the other.  

Example 2:  
Input: graph = [[1,3],[0,2],[1,3],[0,2]]  
Output: true  
Explanation: We can partition the nodes into two sets: {0, 2} and {1, 3}.  
  

Constraints:

graph.length == n
1 <= n <= 100
0 <= graph[u].length < n
0 <= graph[u][i] <= n - 1
graph[u] does not contain u.
All the values of graph[u] are unique.
If graph[u] contains v, then graph[v] contains u.

## Approach - 
using bfs, at each non-visited node, check if its adjacent node is stored in the same location

```
class Solution {
    public boolean isBipartite(int[][] graph) {
         // suppose there are two set - 0 and 1. Here the array "loc" will store the number of set of ith node
        int loc[]=new int[graph.length];
        Arrays.fill(loc,-1); // we have to fill it by -1, beacause initially empty array will give 0 value at each index
        for(int i=0; i<graph.length; i++){
            if(graph[i].length > 0 && loc[i]==-1){
                if(!rec(graph,i,loc)){
                    return false;
                }
            }
        }
        return true;
    }
    private boolean rec(int graph[][], int start, int loc[]){
        Queue<Integer> q=new LinkedList<>();
        q.add(start);
        loc[start]=0;
        while(!q.isEmpty()){
            // get the current node
            int node=q.remove();
            // check if any of its neighour is in the same set as currentNode is.
            for(int adjNode : graph[node]){
                if(loc[adjNode]==loc[node]){ return false; } // if both are in same set
                else if(loc[adjNode]==-1){
                    loc[adjNode]=1-loc[node];
                    q.add(adjNode);
                }
            }
        }
        return true;
    }
}
