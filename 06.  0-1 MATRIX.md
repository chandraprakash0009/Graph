## 542. 01 Matrix

Given an m x n binary matrix mat, return the distance of the nearest 0 for each cell.  
The distance between two cells sharing a common edge is 1.  

Example 1:  

Input: mat = [[0,0,0],[0,1,0],[0,0,0]]  
Output: [[0,0,0],[0,1,0],[0,0,0]]  

Example 2:  
Input: mat = [[0,0,0],[0,1,0],[1,1,1]]
Output: [[0,0,0],[0,1,0],[1,2,1]]
 

Constraints:

m == mat.length
n == mat[i].length
1 <= m, n <= 104
1 <= m * n <= 104
mat[i][j] is either 0 or 1.
There is at least one 0 in mat.

## Approach - 
in queue, initially add all pair(row,col,0) where element is equal to 0.  
FROM each pair - find all four direction -
1.  if that cell is already visited then check for min distance
2.  else update the cell by cur+1 and add that cell in queue
```class Solution {
    class Pair{
        int row, col, dist;
        Pair(int r, int c,int d){ row=r; col=c; dist=d; }
    }
    public int[][] updateMatrix(int[][] mat) {
        int m=mat.length, n=mat[0].length;
        Queue<Pair> q = new LinkedList<>();
        boolean vis[][]=new boolean[m][n];
        int nums[][]=new int[m][n];
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                if(mat[i][j]==0){
                    q.add(new Pair(i,j,0));
                    vis[i][j]=true;
                }
            }
        }
        int dr[]={0,1,0,-1};
        int dc[]={1,0,-1,0};
        while(!q.isEmpty()){
            Pair pair=q.remove();
            int r=pair.row, c=pair.col, d=pair.dist;
            for(int i=0; i<4; i++){
                int nrow=r+dr[i], ncol=c+dc[i];
                if(nrow>=0 && nrow<m && ncol>=0 && ncol<n){
                    if(vis[nrow][ncol]){
                        nums[nrow][ncol]=Math.min(nums[nrow][ncol], d+1);
                    }else{
                        vis[nrow][ncol]=true;
                        nums[nrow][ncol]=d+1;
                        q.add(new Pair(nrow,ncol,nums[nrow][ncol]));
                    }
                }
            }
        }
        return nums;
    }
}
