## Shortest Path in Weighted undirected graph

You are given a weighted undirected graph having n vertices numbered from 1 to n and m edges along with their weights. Find the shortest weight path between the vertex 1 and the vertex n,  if there exists a path, and return a list of integers whose first element is the weight of the path, and the rest consist of the nodes on that path. If no path exists, then return a list containing a single element -1.

The input list of edges is as follows - {a, b, w}, denoting there is an edge between a and b, and w is the weight of that edge.

Note: The driver code here will first check if the weight of the path returned is equal to the sum of the weights along the nodes on that path, if equal it will output the weight of the path, else -2. In case the list contains only a single element (-1) it will simply output -1. 

Examples :  

Input: n = 5, m= 6, edges = [[1, 2, 2], [2, 5, 5], [2, 3, 4], [1, 4, 1], [4, 3, 3], [3, 5, 1]]  
Output: 5  
Explanation: Shortest path from 1 to n is by the path 1 4 3 5 whose weight is 5.   
Input: n = 2, m= 1, edges = [[1, 2, 2]]  
Output: 2  
Explanation: Shortest path from 1 to 2 is by the path 1 2 whose weight is 2.   
Input: n = 2, m= 0, edges = [ ]  
Output: -1  
Explanation: Since there are no edges, so no answer is possible.  
Expected Time Complexity: O(m* log(n))  
Expected Space Complexity: O(n+m)  

Constraint:   
2 <= n <= 106  
0 <= m <= 106  
1 <= a, b <= n  
1 <= w <= 10  

##  Approach - 
Using Dijikstra Algorithm, find the minimum weight of each node.  
While updating the weight of adjacent node , keep an array to keep track of parent of each node.  
After all, check if nth node is visited or not.
1.  if visited, then return list with a single value -1.
2.  add nth node in the list, and update node with the parent of node because we have reached to the current node form parent[currentNode].
In this way, we can track the path from first node to nth node

```
class Solution {
    class Pair{
        int nod, wt;
        Pair(int n,int w){
            nod=n; wt=w;
        }
    }
    public List<Integer> shortestPath(int n, int m, int edges[][]) {
        //  Code Here.
        List<List<Integer>> adj = new ArrayList<>();
        int size=edges.length;
        for(int i=0; i<n+1; i++){
            adj.add(new ArrayList<>());
        }
        HashMap<String,Integer> map=new HashMap<>();
        for(int nums[] : edges){
            adj.get(nums[0]).add(nums[1]);
            map.put((nums[0]+","+nums[1]), nums[2]);
            
            adj.get(nums[1]).add(nums[0]);
            map.put((nums[1]+","+nums[0]), nums[2]);
        }
        int weight[]=new int[n+1];
        Arrays.fill(weight,Integer.MAX_VALUE);
        weight[1]=0;
        int parent[]=new int[n+1];
        parent[1]=1;
        PriorityQueue<Pair> pq=new PriorityQueue<>((a,b)->Integer.compare(a.wt,b.wt));
        pq.add(new Pair(1,0));
        while(!pq.isEmpty()){
            int node=pq.peek().nod;
            int curwt=pq.peek().wt;
            pq.remove();
            for(int adjNode : adj.get(node)){
                int temp=map.get(node+","+adjNode);
                if(curwt+temp<weight[adjNode]){
                    weight[adjNode]=temp+curwt;
                    parent[adjNode]=node;
                    pq.add(new Pair(adjNode,weight[adjNode]));
                }
            }
        }
        List<Integer> ans=new ArrayList<>();
        if(weight[n]==Integer.MAX_VALUE){
            ans.add(-1);
            return ans;
        }
        int node=n;
        while(parent[node]!=node){
            ans.add(0,node);
            node=parent[node];
        }
        ans.add(0,1);
        ans.add(0,weight[n]);
        return ans;
    }
}
