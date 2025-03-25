## 994. Rotting Oranges

You are given an m x n grid where each cell can have one of three values:  

0 representing an empty cell,  
1 representing a fresh orange, or  
2 representing a rotten orange.  
Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.  

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.  

 

Example 1:  


Input: grid = [[2,1,1],[1,1,0],[0,1,1]]    
Output: 4  
Example 2:  

Input: grid = [[2,1,1],[0,1,1],[1,0,1]]  
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.  
Example 3:

Input: grid = [[0,2]]
Output: 0
Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 10
grid[i][j] is 0, 1, or 2.

### APPROACH - 
using queue(bfs), store all the rooten fruit initially. if any fruit is rooten at any index at time t then all the fresh fruits adjacent to it will be rooten in t+1 second.

```class Solution {
    class Pair{
        int row, col, tim;
        Pair(int r, int c, int t){ row=r; col=c; tim=t; }
    }
    public int orangesRotting(int[][] grid) {
        int m=grid.length, n=grid[0].length;
        int vis[][]=new int[m][n];
        int fresh=0; // number of fresh orange initially
        Queue<Pair> q= new LinkedList<>();
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(grid[i][j]==2){
                    q.add(new Pair(i,j,0));
                    vis[i][j]=2;
                }else{
                    vis[i][j]=0;
                }
                if(grid[i][j]==1){ fresh++; }
            }
        }
        int dr[]={0,1,0,-1};
        int dc[]={1,0,-1,0};
        int time=0;
        while(!q.isEmpty()){
            Pair item = q.remove();
            int r=item.row, c=item.col, tm=item.tim;
            time=Math.max(time,tm);
            for(int i=0; i<4; i++){
                int  nrow=r+dr[i], ncol=c+dc[i];
                if(nrow>=0 && nrow<m && ncol>=0 && ncol<n && grid[nrow][ncol]==1 && vis[nrow][ncol]==0){
                    q.add(new Pair(nrow,ncol,tm+1));
                    fresh--;
                    vis[nrow][ncol]=2;
                }
            }
        }
        return (fresh==0)?time:-1;
    }
}
