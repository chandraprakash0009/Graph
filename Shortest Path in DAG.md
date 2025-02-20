## Shortest path in Directed Acyclic Graph

Given a Directed Acyclic Graph of V vertices from 0 to n-1 and a 2D Integer array(or vector) edges[ ][ ] of length E, where there is a directed edge from edge[i][0] to edge[i][1] with a distance of edge[i][2] for all i.

Find the shortest path from src(0) vertex to all the vertices and if it is impossible to reach any vertex, then return -1 for that vertex.

Examples :

Input: V = 4, E = 2, edges = [[0,1,2], [0,2,1]]  
Output: [0, 2, 1, -1]  
Explanation: Shortest path from 0 to 1 is 0->1 with edge weight 2. Shortest path from 0 to 2 is 0->2 with edge weight 1. There is no way we can reach 3, so it's -1 for 3.  

Input: V = 6, E = 7, edges = [[0,1,2], [0,4,1], [4,5,4], [4,2,2], [1,2,3], [2,3,6], [5,3,1]]  
Output: [0, 2, 3, 6, 1, 5]  
Explanation: Shortest path from 0 to 1 is 0->1 with edge weight 2. Shortest path from 0 to 2 is 0->4->2 with edge weight 1+2=3. Shortest path from 0 to 3 is 0->4->5->3 with edge weight 1+4+1=6. Shortest path from 0 to 4 is 0->4 with edge weight 1.Shortest path from 0 to 5 is 0->4->5 with edge weight 1+4=5.  
Constraint:  
1 <= V <= 100  
1 <= E <= min((N*(N-1))/2,4000)  
0 <= edgesi,0, edgesi,1 < n  
0 <= edgei,2 <=105  

```

// User function Template for Java
class Solution {
    class Pair{
        int nod, wt;  // node means node number and wt means weight from 0 to current node
        Pair(int n, int w){
            nod=n; wt=w;
        }
    }
    public int[] shortestPath(int V, int E, int[][] edges) {
        // Code here
        // adj list to find the adjacent node
        List<List<Integer>> adj=new ArrayList<>();
        for(int i=0; i<V; i++){
            adj.add(new ArrayList<>());
        }
        // if u->v costs w then map store "u v" as key and w as value
        HashMap<String,Integer> map = new HashMap<>();
        for(int i=0; i<edges.length; i++){
            int u=edges[i][0];
            int v=edges[i][1];
            int w=edges[i][2];
            adj.get(u).add(v);
            String key=u+" "+v;
            map.put(key,w);
        }
        // each time we need the non-visited nde with smallest weight
        PriorityQueue<Pair> pq = new PriorityQueue<>((a,b)->Integer.compare(a.wt,b.wt));
        // weight array initially store the weight from source node(0) to ith node at the ith index. In the very beggining, weight[0]=0 beacause of starting node
        int weight[]=new int[V];
        // since this array store smallest weight, so initialize with large value
        Arrays.fill(weight,10000000);
        weight[0]=0;
        
        // priority queue stores only unvisited node
        pq.add(new Pair(0,0));

        while(!pq.isEmpty()){
            //count++;
            Pair pair = pq.remove();
            int curWt = pair.wt;
            int node=pair.nod;
            // update the weight of adjacent node if not visited
            for(int adjNode : adj.get(node)){
                // check if the updated weight is smaller than previous one
                String key = node + " " + adjNode;
                if(curWt+map.get(key) < weight[adjNode]){
                    weight[adjNode]=curWt + map.get(key);
                    pq.add(new Pair(adjNode,weight[adjNode]));
                }
            }
        }
        // mark -1 for those node which could not get visited
        for(int i=0; i<V; i++){
            if(weight[i]==10000000){
                weight[i]=-1;
            }
        }
        return weight;
    }
}
