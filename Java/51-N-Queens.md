# N-Queens

> The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

n皇后问题，给一个n x n的棋盘，n个皇后，判断有多少种摆法使得皇后直接不会互相攻击。

## 解题思路

尝试画图来看一下。一个n x n的棋盘有2 * n-1 条对角线。

三个布尔数组来表示，一个格子能否被使用。分别对于一列，正对角线，反对角线。

正对角线以左上角为0,向右下走，每个格子对应的为`rowIndex+i`。反对角线以右上角为0,向左下走，每个格子对应的为`rowIndex-i+n-1`。

rowIndex 表示行，for循环 i 表示列。

注意递归调用fun后的 reset 操作，不然会影响下一行。

时间复杂度O(n!)

```Java
class Solution {
    List<List<String>> result = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {
        boolean[] visited = new boolean[n];
        //2*n-1个斜对角线
        boolean[] dia1 = new boolean[2*n-1];
        boolean[] dia2 = new boolean[2*n-1];
        
        fun(n, new ArrayList<String>(),visited,dia1,dia2,0);
        
        return result;
    }
    
    private void fun(int n,List<String> list,boolean[] visited,boolean[] dia1,boolean[] dia2,int rowIndex){
        if(rowIndex == n){
            result.add(new ArrayList<String>(list));
            return;
        }
        
        for(int i=0;i<n;i++){
            //这一列、正对角线、反对角线都不能再放了，如果发现是true，停止本次循环
            if(visited[i] || dia1[rowIndex+i] || dia2[rowIndex-i+n-1])
                continue;
            
            //init一个长度为n的一维数组，里面初始化为'.'
            char[] charArray = new char[n];
            Arrays.fill(charArray,'.');
            
            charArray[i] = 'Q';
            String stringArray = new String(charArray);
            list.add(stringArray);
            visited[i] = true;
            dia1[rowIndex+i] = true;
            dia2[rowIndex-i+n-1] = true;

            fun(n,list,visited,dia1,dia2,rowIndex+1);

            //reset 不影响回溯的下个目标
            list.remove(list.size()-1);
            charArray[i] = '.';
            visited[i] = false;
            dia1[rowIndex+i] = false;
            dia2[rowIndex-i+n-1] = false;
        }
    }
    
}
```