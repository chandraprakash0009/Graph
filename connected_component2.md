## 200. Number of Islands

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.  

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically.  
You may assume all four edges of the grid are all surrounded by water.  

 

Example 1:  

Input: grid = [  
  ["1","1","1","1","0"],  
  ["1","1","0","1","0"],  
  ["1","1","0","0","0"],  
  ["0","0","0","0","0"]  
]  
Output: 1  
Example 2:  

Input: grid = [  
  ["1","1","0","0","0"],  
  ["1","1","0","0","0"],  
  ["0","0","1","0","0"],  
  ["0","0","0","1","1"]  
]  
Output: 3  
 

Constraints:  

m == grid.length  
n == grid[i].length  
1 <= m, n <= 300  
grid[i][j] is '0' or '1'.  

### APPRAOCH - find the number of connected component. mark each adjacent node with "1" as visited which is adjacent to a node with value "1"

``` class Solution {
    int dr[]={0,1,0,-1};
    int dc[]={1,0,-1,0};
    public int numIslands(char[][] grid) {
        int ans=0;
        //boolean vis[][]=new boolean[grid.length][grid[0].length];
        for(int i=0; i<grid.length; i++){
            for(int j=0; j<grid[0].length; j++){
                if(grid[i][j]=='1' ){
                    ans++;
                    rec(grid,i,j);
                }
            }
        }
        return ans;
    }
    private void rec(char grid[][], int r, int c){
        int row=grid.length;
        int col=grid[0].length;
        if(r<0 || c<0 || r>=row || c>=col || grid[r][c]=='0'){ return; }
        grid[r][c]='0';
        for(int i=0; i<4; i++){
            rec(grid,r+dr[i],c+dc[i]);
        }
    }
}```
