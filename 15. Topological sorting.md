## Topological sorting
Here’s a step-by-step algorithm for topological sorting using Depth First Search (DFS):

1.  Create a graph with n vertices and m-directed edges.
2.  Initialize a stack and a visited array of size n.
3.  For each unvisited vertex in the graph, do the following:
      1.  Call the DFS function with the vertex as the parameter.
      2.  In the DFS function, mark the vertex as visited and recursively call the DFS function for all unvisited neighbors of the vertex.
      3.  Once all the neighbors have been visited, push the vertex onto the stack.
4.  After all, vertices have been visited, pop elements from the stack and append them to the output list until the stack is empty.
5.  The resulting list is the topologically sorted order of the graph.
```

class Solution {
    // Function to return list containing vertices in Topological order.
    static ArrayList<Integer> topologicalSort(ArrayList<ArrayList<Integer>> adj) {
        // Your code here
        Stack<Integer> st=new Stack<>();
        boolean vis[]=new boolean[adj.size()];
        for(int i=0; i<adj.size(); i++){
            if(!vis[i]){
                dfs(adj,i,st,vis);
            }
        }
        ArrayList<Integer> ans=new ArrayList<>();
        while(!st.isEmpty()){
            ans.add(st.pop());
        }
        return ans;
    }
    private static void dfs(ArrayList<ArrayList<Integer>> adj,int node,Stack<Integer> st, boolean vis[]){
        vis[node]=true;
        for(int adjNode : adj.get(node)){
            if(!vis[adjNode]){
                dfs(adj,adjNode,st,vis);
            }
        }
        st.push(node);
    }
}
