# Valid Sudoku

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits 1-9 without repetition.
2. Each column must contain the digits 1-9 without repetition.
3. Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

验证数独，
每一行都只有1-9不重复。
每一列都只有1-9不重复。
每个九宫格都只有1-9不重复。

## 解题思路

这一题不要陷入误区，不要考虑那些空着的格子，只考虑已经存在的数字即可。

对存在的数字，进行验证，验证条件就是上面列出的三行。

行和列都容易验证，
九宫格的表示方法的话，我们可以用九宫格左上角的那个元素，作为前缀，加上当前元素，存到set中，当add失败返回false，即可知存在重复，不符合严重条件。

```Java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        Set seen = new HashSet();
        for (int i=0; i<9; ++i) {
            for (int j=0; j<9; ++j) {
                char number = board[i][j];
                if (number != '.')
                    if (!seen.add(number + " in row " + i) ||
                        !seen.add(number + " in column " + j) ||
                        !seen.add(number + " in block " + i/3 + "-" + j/3))
                        return false;
            }
        }
        return true;
    }
}
```