## Topologicak sorting Using BFS
If there is no cycle in the directed graph, then topological sorting ensures that eachy node would be visited. 
```
class Solution {
    // Function to return list containing vertices in Topological order.
    static ArrayList<Integer> topologicalSort(ArrayList<ArrayList<Integer>> adj) {
        // Your code here
        int v=adj.size();
        int indegree[]=new int[v];
        for(int node=0; node<v; node++){
            for(int adjNode : adj.get(node)){
                indegree[adjNode]++;
            }
        }
        Queue<Integer> q= new LinkedList<>();
        for(int i=0; i<v; i++){
            if(indegree[i]==0){
                q.add(i);
            }
        }
        ArrayList<Integer>  ans = new ArrayList<>();
        while(!q.isEmpty()){
            int node=q.remove();
            ans.add(node);
            for(int adjNode : adj.get(node)){
                indegree[adjNode]--;
                if(indegree[adjNode]==0){
                    q.add(adjNode);
                }
            }
        }
        return ans;
    }
}
