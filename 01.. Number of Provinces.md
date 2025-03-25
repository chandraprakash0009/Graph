## 547. Number of Provinces

There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

 

Example 1:


Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2

Example 2:


Input: isConnected = [[1,0,0],[0,1,0],[0,0,1]]
Output: 3
 

Constraints:  
 
1 <= n <= 200  
n == isConnected.length  
n == isConnected[i].length  
isConnected[i][j] is 1 or 0.  
isConnected[i][i] == 1  
isConnected[i][j] == isConnected[j][i]  

## APPROACH : FOR EACH NODE, MARK ALL NODE AS VISITED WHICH IS CONNECTED TO CURRENT NODE

```class Solution {
    public int findCircleNum(int[][] isConnected) {
        int row=isConnected.length;
        int col=isConnected[0].length;
        int ans=0;
        boolean vis[]=new boolean[row];
        for(int i=0; i<row; i++){
            if(!vis[i]){ 
                vis[i]=true; ans++;
                rec(isConnected,i,vis);
            }
        }
        return ans;
    }
    private void rec(int nums[][], int node, boolean vis[]){
        for(int i=0; i<nums[0].length; i++){
            if(i!=node && nums[node][i]==1){
                if(!vis[i]){
                    vis[i]=true;
                    rec(nums,i,vis);
                }
            }
        }
    }
}```
