## Number of Distinct Islands 
Given a boolean 2D matrix grid of size n * m. You have to find the number of distinct islands where a group of connected 1s (horizontally or vertically) forms an island.  
Two islands are considered to be distinct if and only if one island is not equal to another (not rotated or reflected).  

Example 1:  
  
Input:  
grid[][] = {{1, 1, 0, 0, 0},  
            {1, 1, 0, 0, 0},  
            {0, 0, 0, 1, 1},  
            {0, 0, 0, 1, 1}}  
Output:  1  
Explanation:  
grid[][] = {{1, 1, 0, 0, 0},   
            {1, 1, 0, 0, 0},   
            {0, 0, 0, 1, 1},   
            {0, 0, 0, 1, 1}}  
Same colored islands are equal.  
We have 2 equal islands, so we   
have only 1 distinct island.  

Example 2:  

Input:  
grid[][] = {{1, 1, 0, 1, 1},  
            {1, 0, 0, 0, 0},  
            {0, 0, 0, 0, 1},  
            {1, 1, 0, 1, 1}}  
Output: 3  
Explanation:  
grid[][] = {{1, 1, 0, 1, 1},   
            {1, 0, 0, 0, 0},   
            {0, 0, 0, 0, 1},   
            {1, 1, 0, 1, 1}}  
Same colored islands are equal.  
We have 4 islands, but 2 of them  are equal, So we have 3 distinct islands.  

Your Task:  
You don't need to read or print anything. Your task is to complete the function countDistinctIslands() which takes the grid as an input parameter and   
returns the total number of distinct islands.  

Expected Time Complexity: O(n * m)  
Expected Space Complexity: O(n * m)  
 
Constraints:  
1 ≤ n, m ≤ 500  
grid[i][j] == 0 or grid[i][j] == 1  

## Appraoch - 
Since we have to find distinct island , it means the shape of each island should be different.
how can we classify different shape ??  
1. what if we store the index of each shape in the form of string with respect to first cell(here first cell mean top-leftmost cell  i.e. (0,0)).
By doing so, each shape would be shown as it is starting from very first cell.
2.  how would we represent shape with respect to first cell ??
from each index of shape including starting index of the shape, if we subtract starting row number and column number, the starting index become (0,0).
In this way we can store the shape.
to avoid repeatation, store the list of index of shape in set.

```

class Solution {
    int dr[]={0,1,0,-1};
    int dc[]={1,0,-1,0};
    int countDistinctIslands(int[][] grid) {
       int m=grid.length, n=grid[0].length;
        boolean vis[][]=new boolean[m][n];
        Set<List<String>> set=new HashSet<>();
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(!vis[i][j] && grid[i][j]==1){
                    List<String> list=new ArrayList<>();
                    rec(i,j,vis,grid,i,j,list);
                    set.add(list);
                }
            }
        }
        return set.size();
    }
    private void rec(int r, int c, boolean vis[][],int grid[][],int startrow, int startcol,List<String> list){
        vis[r][c]=true;
        list.add(findString(r-startrow,c-startcol));
        int m=grid.length, n=grid[0].length;
        for(int i=0; i<4; i++){
            int nrow=r+dr[i];
            int ncol=c+dc[i];
            if(nrow>=0 && nrow<m && ncol>=0 && ncol<n){
                if(!vis[nrow][ncol] && grid[nrow][ncol]==1){
                    rec(nrow,ncol,vis,grid,startrow,startcol,list);
                }
            }
        }
    }
    private String findString(int a, int b){
        return Integer.toString(a) + " " + Integer.toString(b);
    }
}
