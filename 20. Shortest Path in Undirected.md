## Shortest Path in Undirected

You are given an adjacency list, adj of Undirected Graph having unit weight of the edges, find the shortest path from src to all the vertex and if it is unreachable to reach any vertex, then return -1 for that vertex.

Examples :

Input: adj[][] = [[1, 3], [0, 2], [1, 6], [0, 4], [3, 5], [4, 6], [2, 5, 7, 8], [6, 8], [7, 6]], src=0  
Output: [0, 1, 2, 1, 2, 3, 3, 4, 4]  

Input: adj[][]= [[3], [3], [], [0, 1]], src=3  
Output: [1, 1, -1, 0]  

Input: adj[][]= [[], [], [], [4], [3], [], []], src=1  
Output: [-1, 0, -1, -1, -1, -1, -1]   
Constraint:  
1<=adj.size()<=104   
0<=edges<=adj.size()-1  

## Approach - 
Using bfs, initialize the weight of each node with the large value (consider as infinite);
Mark the source node with with weight 0 and add the source node in the queue.
for each adjacent node of current node, check if the weight of current node +1 is less than the weight of adjacent node-
if lesser then update the weight of adjacent node and add it to the queue.

```

class Solution {
    
    // Function to find the shortest path from a source node to all other nodes
    public int[] shortestPath(ArrayList<ArrayList<Integer>> adj, int src) {
        // code here
        int weight[]=new int[adj.size()];
        Arrays.fill(weight,10000000);
        weight[src]=0;
        Queue<Integer> q=new LinkedList<>();
        q.add(src);
        while(!q.isEmpty()){
            int node=q.remove();
            for(int adjNode : adj.get(node)){
                if(weight[node]+1 < weight[adjNode]){
                    weight[adjNode]=weight[node]+1;
                    q.add(adjNode);
                }
            }
        }
        for(int i=0; i<weight.length; i++){
            if(weight[i]==10000000){
                weight[i]=-1;
            }
        }
        return weight;
    }
}

