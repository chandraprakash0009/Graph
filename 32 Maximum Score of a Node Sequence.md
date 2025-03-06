## 2242. Maximum Score of a Node Sequence

There is an undirected graph with n nodes, numbered from 0 to n - 1.

You are given a 0-indexed integer array scores of length n where scores[i] denotes the score of node i. You are also given a 2D integer array edges where edges[i] = [ai, bi] denotes that there exists an undirected edge connecting nodes ai and bi.

A node sequence is valid if it meets the following conditions:

There is an edge connecting every pair of adjacent nodes in the sequence.
No node appears more than once in the sequence.
The score of a node sequence is defined as the sum of the scores of the nodes in the sequence.

Return the maximum score of a valid node sequence with a length of 4. If no such sequence exists, return -1.

 

Example 1:

Input: scores = [5,2,9,8,4], edges = [[0,1],[1,2],[2,3],[0,2],[1,3],[2,4]]
Output: 24
Explanation: The figure above shows the graph and the chosen node sequence [0,1,2,3].
The score of the node sequence is 5 + 2 + 9 + 8 = 24.
It can be shown that no other node sequence has a score of more than 24.
Note that the sequences [3,1,2,0] and [1,0,2,3] are also valid and have a score of 24.
The sequence [0,3,2,4] is not valid since no edge connects nodes 0 and 3.  

Example 2:

Input: scores = [9,20,6,4,11,12], edges = [[0,3],[5,3],[2,4],[1,3]]
Output: -1
Explanation: The figure above shows the graph.
There are no valid node sequences of length 4, so we return -1.
 

Constraints:

n == scores.length
4 <= n <= 5 * 104
1 <= scores[i] <= 108
0 <= edges.length <= 5 * 104
edges[i].length == 2
0 <= ai, bi <= n - 1
ai != bi
There are no duplicate edges.


### Approach

If we fix the two middle node then the other two node will be fixed automatically.   
There is a cache in this logic and i.e. if we check each adjacent node of both middle node then it would be TLE ( or space problem in memoization) .
So, we will consider only the adjacent node which has high score and maximum three adjacent node with max score is sufficient.

So following the above approach - 

Make graph and sort each each list in the graph based on the score of node in decreasing order.  
Traverse each node given ( since each edge is unique). By doing this, the middle node would be fixed. So now , we will find adjacent node from each two node. 

### Java code
```
class Solution {
    public int maximumScore(int[] scores, int[][] edges) {
        int n = scores.length;
       
        List<List<Integer>> adj = new ArrayList<>();
        
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }

        // Construct adjacency list
        for (int[] edge : edges) {
            int u = edge[0], v = edge[1];
            adj.get(u).add(v);
            adj.get(v).add(u);
        }

        // Sort each adjacency list based on node scores in descending order
        for (List<Integer> list : adj) {
            list.sort((a, b) -> Integer.compare(scores[b], scores[a]));
        }

        int max = -1;

        // Iterate over all node pairs (edges)
        for(int i=0; i<edges.length; i++){
            int a = edges[i][0];
            int b = edges[i][1];

            for(int j=0; j<3; j++){
                if(j>=adj.get(a).size()){ continue; }
                int c = adj.get(a).get(j);

                for(int k=0; k<3; k++){
                    if(k>= adj.get(b).size()){ continue; }
                    int d = adj.get(b).get(k);
                    if(c!=b && c!=d && a!=d){
                        max = Math.max(max, scores[a]+scores[b]+scores[c]+scores[d]);
                    }
                }
            }
        }
        return max;
    }
}


