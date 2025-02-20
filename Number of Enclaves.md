## 1020. Number of Enclaves

You are given an m x n binary matrix grid, where 0 represents a sea cell and 1 represents a land cell.  
A move consists of walking from one land cell to another adjacent (4-directionally) land cell or walking off the boundary of the grid.  
Return the number of land cells in grid for which we cannot walk off the boundary of the grid in any number of moves.  

Example 1:  

Input: grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]  
Output: 3  
Explanation: There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.  

Example 2:  
Input: grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]  
Output: 0  
Explanation: All 1s are either on the boundary or can reach the boundary.  
 

Constraints:

m == grid.length
n == grid[i].length
1 <= m, n <= 500
grid[i][j] is either 0 or 1.

## Approach - 
since we have to walk from land cell to the boundary land cell. We can only reach to boundary if any boundary land cell is connected to the inner land cell directly or indirectly.
So instead of walking from inner to boundary cell , walk from boundary to inner cell using bfs algorithm (using queue). the land which could not get visited, will be our answer.

```
class Solution {
    class Pair{
        int row, col;
        Pair(int r, int c){
            row=r; col=c;
        }
    }
    public int numEnclaves(int[][] grid) {
        Queue<Pair> q=new LinkedList<>();
        int m=grid.length, n=grid[0].length;
        // first and last row
        for(int i=0; i<n; i++){
            if(grid[0][i]==1){
                grid[0][i]=0;
                q.add(new Pair(0,i));
            }
            if(grid[m-1][i]==1){
                grid[m-1][i]=0;
                q.add(new Pair(m-1,i));
            }
        }
        // first and last col
        for(int i=0; i<m; i++){
            if(grid[i][0]==1){
                grid[i][0]=0; q.add(new Pair(i,0));
            }
            if(grid[i][n-1]==1){
                grid[i][n-1]=0; q.add(new Pair(i,n-1));
            }
        }
        int dr[]={0,1,0,-1};
        int dc[]={1,0,-1,0};
        while(!q.isEmpty()){
            int r=q.peek().row;
            int c=q.peek().col;
            q.remove();
            for(int i=0; i<4; i++){
                int nrow=r+dr[i];
                int ncol=c+dc[i];
                if(nrow>=0 && nrow<m && ncol>=0 && ncol<n){
                    if(grid[nrow][ncol]==1){
                        grid[nrow][ncol]=0;
                        q.add(new Pair(nrow,ncol));
                    }
                }
            }
        }
        int count=0;
        for(int i=1; i<m-1; i++){
            for(int j=1; j<n-1; j++){
                count+=grid[i][j];
            }
        }
        return count;
    }
}
