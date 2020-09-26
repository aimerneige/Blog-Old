---
title: LeetCode Surrounded Regions
date: 2020-08-30 10:50:13
tags: LeetCode
category: Program
---
# Surrounded Regions

> https://leetcode.com/problems/surrounded-regions/

> Given a 2D board containing `'X'` and `'O'` (**the letter O**), capture all regions surrounded by `'X'`.
>
> A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.
>
> **Example:**
>
> ```
> X X X X
> X O O X
> X X O X
> X O X X
> ```
>
> After running your function, the board should be:
>
> ```
> X X X X
> X X X X
> X X X X
> X O X X
> ```
>
> **Explanation:**
>
> Surrounded regions shouldn’t be on the border, which means that any `'O'` on the border of the board are not flipped to `'X'`. Any `'O'` that is not on the border and it is not connected to an `'O'` on the border will be flipped to `'X'`. Two cells are connected if they are adjacent cells connected horizontally or vertically.

```c++
class Solution {
public:
    void solve(vector<vector<char>>& board) {

        // get the row and col
        int row = board.size();
        if (row == 0) {
            return;
        }
        int col = board[0].size();{
        if (col == 0) {
            return;
        }
        
        // mark all O as A
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'A';
                }
            }
        }

        // check border and set the border A as O
        // up
        for (int i = 0; i < col; i++) {
            if (board[0][i] == 'A') {
                mark_beside(board, 0, i);
            }
        }
        // down
        for (int i = 0; i < col; i++) {
            if (board[row - 1][i] == 'A') {
                mark_beside(board, row - 1, i);
            }
        }
        // left
        for (int i = 0; i < row; i++) {
            if (board[i][0] == 'A') {
                mark_beside(board, i, 0);
            }
        }
        // right{
        for (int i = 0; i < row; i++) {
            if (board[i][col - 1] == 'A') {
                mark_beside(board, i, col - 1);
            }
        }

        // mark all other A as X
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] == 'A') {
                    board[i][j] = 'X';
                }
            }
        }
    }

    void mark_beside(vector<vector<char>>& board, int i, int j) {
        // mark
        board[i][j] = 'O';
        // check up
        if (i > 0) {
            if (board[i - 1][j] == 'A') {
                mark_beside(board, i - 1, j);
            }
        }
        // check down
        if (i < board.size() - 1) {
            if (board[i + 1][j] == 'A') {
                mark_beside(board, i + 1, j);
            }
        }
        // check left
        if (j > 0) {
            if (board[i][j - 1] == 'A') {
                mark_beside(board, i, j - 1);
            }
        }
        // check right
        if (j < board[0].size() - 1) {
            if (board[i][j + 1] == 'A') {
                mark_beside(board, i, j + 1);
            }
        }
    }
};

/*
 * A  means not been checked
 * X  means X or be surrounded
 * 0  means border or can't be surrounded
 */
```

```java
class Solution {
    void mark(char[][] board, int i, int j) {
        board[i][j] = 'O';
        if (i > 0) {
            if (board[i - 1][j] == 'A') {
                mark(board, i - 1, j);
            }
        }
        if (i < board.length - 1) {
            if (board[i + 1][j] == 'A') {
                mark(board, i + 1, j);
            }
        }
        if (j > 0) {
            if (board[i][j - 1] == 'A') {
                mark(board, i, j - 1);
            }
        }
        if (j < board[0].length - 1) {
            if (board[i][j + 1] == 'A') {
                mark(board, i, j + 1);
            }
        }
    }

    public void solve(char[][] board) {
        int row = board.length;
        if (row == 0) {
            return;
        }
        int col = board[0].length;
        if (col == 0) {
            return;
        }

        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'A';
                }
            }
        }

        for (int i = 0; i < col; i++) {
            if (board[0][i] == 'A') {
                mark(board, 0, i);
            }
            if (board[row - 1][i] == 'A') {
                mark(board, row - 1, i);
            }
        }
        for (int i = 0; i < row; i++) {
            if (board[i][0] == 'A') {
                mark(board, i, 0);
            }
            if (board[i][col - 1] == 'A') {
                mark(board, i, col - 1);
            }
        }
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                if (board[i][j] == 'A') {
                    board[i][j] = 'X';
                }
            }
        }
    }
}
```
