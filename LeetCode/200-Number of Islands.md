# Number of Islands

> Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

土地上下左右相连的算一个岛，求岛的数量。

## 解题思路

就是一个图的联通块问题

一个很棒的解法

```Java
public class Solution {
    char[][] g;
    public int numIslands(char[][] grid) {
        int islands = 0;
        g = grid;
        for (int i=0; i<g.length; i++)
            for (int j=0; j<g[i].length; j++)
                islands += sink(i, j);
        return islands;
    }
    int sink(int i, int j) {
        if (i < 0 || i == g.length || j < 0 || j == g[i].length || g[i][j] == '0')
            return 0;
        g[i][j] = '0';
        sink(i+1, j); sink(i-1, j); sink(i, j+1); sink(i, j-1);
        return 1;
    }
}
```

我这里用一个二维布尔数组，表示某块土地是否已经被遍历到了。
```Java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) return 0;
        int row = grid.length;
        int col = grid[0].length;

        boolean[][] used = new boolean[row][col];

        int count = 0;

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (grid[i][j] == '1' && !used[i][j]) {
                    count += 1;
                    dfs(grid, used, i, j);
                }
            }
        }

        return count;
    }

    public void dfs(char[][] grid, boolean[][] used, int r, int c) {
        if (used[r][c]) return;

        if (grid[r][c] == '1') {
            used[r][c] = true;
            // 上下
            if (r - 1 >= 0) {
                dfs(grid, used, r-1, c);
            }
            if (r + 1 < grid.length) {
                dfs(grid, used, r+1, c);
            }
            // 左右
            if (c - 1 >= 0) {
                dfs(grid, used, r, c-1);
            }
            if (c + 1 < grid[0].length) {
                dfs(grid, used, r, c+1);
            }
        } else {
            used[r][c] = true;
            return;
        }
    }
}
```

Python解法
```Python
def numIslands(self, grid):
    def sink(i, j):
        if 0 <= i < len(grid) and 0 <= j < len(grid[i]) and grid[i][j] == '1':
            grid[i][j] = '0'
            map(sink, (i+1, i-1, i, i), (j, j, j+1, j-1))
            return 1
        return 0
    return sum(sink(i, j) for i in range(len(grid)) for j in range(len(grid[i])))
```