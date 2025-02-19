## Cycle in a Directed Graph

Given a Directed Graph with V vertices (Numbered from 0 to V-1) and E edges, check whether it contains any cycle or not.
The graph is represented as an adjacency list, where adj[i] contains a list of vertices that are directly reachable from vertex i. Specifically, adj[i][j] represents an edge from vertex i to vertex j.

Example 1:  

Input:  
Output: 1  
Explanation: 3 -> 3 is a cycle   

Example 2:    
Input:0->1->2  
Output: 0  
Explanation: no cycle in the graph  
Constraints:  
1 ≤ V, E ≤ 105  

## Approach -
Topological sorting is a technique of traversing graph in a directed acyclic graph.  If there is no cycle in the directed graph, then this technique would travrese to each and every node of the graph. 
if there is any cycle , then the traversing would not reach to each node. 
Hence, the nnumber of node occured in the traverse is equal to the number of node int graph then there is no cycle , otherwise cycle is in the graph.

In topological sorting, the node is printed from lowest indegree to highest indegree.
if the indegree of node is 0, (it means that node is of minimum degree in the remaining node), only then that node would be added in the queue.

```

class Solution {
    // Function to detect cycle in a directed graph.
    public boolean isCyclic(ArrayList<ArrayList<Integer>> adj) {
        // code here
        int v=adj.size();
        int indegree[]=new int[v];
        for(int node=0; node<adj.size(); node++){
            for(int adjNode : adj.get(node)){
                indegree[adjNode]++;
            }
        }
        Queue<Integer> q= new LinkedList<>();
        // adding node with the lowest i.e. 0 degree
        for(int i=0; i<v; i++){
            if(indegree[i]==0){
                q.add(i);
            }
        }
        int count=0;
        while(!q.isEmpty()){
            int node=q.remove();
            count++;
            for(int adjNode : adj.get(node)){
                indegree[adjNode]--;
                if(indegree[adjNode]==0){ // if this adjNode is of min degree among remaining nodes
                    q.add(adjNode);
                }
            }
        }
        return count!=v;
    }
}
