---
tags: [state/closed/2022/10]
created: 2026-09-15T13:59:51.000+02:00
modified: 2026-09-23T16:30:54.559+02:00
---

# T2022-1001-2305 Sudoku Solver
#state/closed/2022/10
## Description
Sudoku solver.
A tool to solve sudokus for you.

Some resources and examples are:
<https://en.wikipedia.org/wiki/Sudoku_solving_algorithms>
<https://www.askpython.com/python/examples/sudoku-solver-in-python>
<https://www.linkedin.com/pulse/python-sudoku-solver-klaid-begeja>

- The solution is mainly based on the askpython tutorial (see link above)
- I still don't completely understand the implementation of the backtracking algorithm though...

## Todo

## Done
### 2022-10-01
- Created Task

### 2022-10-10
Solution
```python
# Created 2022-10-01
# Version: 0.1
# Author: ME


# This is the sudoku grid.
# It consists of 9 lists
# Each list contains 9 items
# Each item is a number from 0 to 9. 0 means empty.
# Index (list,number) starting from 0,0 to 8,8
sudoku_grid = [[2, 5, 0, 0, 3, 0, 9, 0, 1],
               [0, 1, 0, 0, 0, 4, 0, 0, 0],
               [4, 0, 7, 0, 0, 0, 2, 0, 8],
               [0, 0, 5, 2, 0, 0, 0, 0, 0],
               [0, 0, 0, 0, 9, 8, 1, 0, 0],
               [0, 4, 0, 0, 0, 3, 0, 0, 0],
               [0, 0, 0, 3, 6, 0, 0, 7, 2],
               [0, 7, 0, 0, 0, 0, 0, 0, 3],
               [9, 0, 3, 0, 0, 0, 6, 0, 4]]
grid_size=len(sudoku_grid)

def print_sudoku(grid):
    for row in range(grid_size):
        if row % 3 == 0:
            print("+-------+-------+-------+")
        for column in range(grid_size):
            if column % 3 == 0:
                print("| ",end="")
            print(grid[row][column],end = " ")
            if column == 8:
                print("|")
    print("+-------+-------+-------+")

def solve(grid,row,col,num):
    for x in range(9):
        if grid[row][x] == num:
            return False

    for x in range(9):
        if grid[x][col] == num:
            return False
    
    row_start=(row//3)*3
    col_start=(col//3)*3
    for row in range(row_start,row_start+3):
        for col in range(col_start,col_start+3):
            if grid[row][col] == num:
                return False
    
    return True

def sudoku(grid,row,col):
    if(row == grid_size - 1 and col == grid_size):
        return True
    if col == grid_size:
        row += 1
        col = 0
    if grid[row][col]>0:
        return sudoku(grid,row,col+1)
    for num in range(1,grid_size+1):
        if solve(grid,row,col,num):
            grid[row][col] = num
            if sudoku(grid,row,col+1):
                return True
        grid[row][col] = 0
    return False

def main():
    print_sudoku(sudoku_grid)
    if (sudoku(sudoku_grid,0,0)):
        print("Solution:")
        print_sudoku(sudoku_grid)

if __name__ == "__main__":
    main()

```
