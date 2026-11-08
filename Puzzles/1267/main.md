# 11/08/2026 - Count Servers that Communicate (Medium)

## First Thoughts
This should be really easy by just using a loop to hit every item in the outter list and then summing every value in the inner lists to get a total number of computers per row. Then we can transpose the list and get the total amount in each column. To avoid double counting we can keep a list of the number of items in each column and then count item by item across the grid.

## Attempt One (Solving)
I did the same as my ideas in First Thoughts, and it worked out fine.
```py
class Solution:
    def countServers(self, grid: list[list[int]]) -> int:
        rows = [sum(row) for row in grid]
        columns = [sum(column) for column in zip(*grid)]

        total = 0
        for x in range(len(grid)):
            for y in range(len(grid[0])):
                if rows[x] > 1 or columns[y] > 1:
                    total += grid[x][y]
        return total
```
This took 24ms to beat 27.04%, and used 21.34MB of memory to beat 89.86%. The time complexity on this is pretty bad which accounts for why it is so slow in comparison. There's also some work that could be done with memory but I won't stress there.


Refactoring to the following brought my runtime down to 19ms to beat 50.14%. 
```py
class Solution:
    def countServers(self, grid: list[list[int]]) -> int:
        rows = [sum(row) for row in grid]
        columns = [sum(column) for column in zip(*grid)]

        total = 0
        for x in range(len(rows)):
            for y in range(len(columns)):
                if rows[x] > 1 or columns[y] > 1:
                    total += grid[x][y]
        return total
```

`rows` and `columns` can't be cut out easily since then the `zip(*grid)` call would need to be made multiple times which is costly. Additionally we can't replace the `range(len(..))` calls without messing up the final `+= grid[x][y]` at the end. I'm content with this answer and don't see a ton that could be done to improve this outside of the random score variation that happens between submissions.

## Final results:
- Tests: 55/55 passed
- Runtime: 19ms (beats 50.14%)
- Memory: 21.42 (beats 65.07%)