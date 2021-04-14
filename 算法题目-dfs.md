## [ 岛屿的最大面积](https://leetcode-cn.com/problems/max-area-of-island/)

给定一个包含了一些 `0` 和 `1` 的非空二维数组 `grid` 。

一个 **岛屿** 是由一些相邻的 `1` (代表土地) 构成的组合，这里的「相邻」要求两个 `1` 必须在水平或者竖直方向上相邻。你可以假设 `grid` 的四个边缘都被 `0`（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 `0` 。

**思路**：对于是1的点，对上下左右四个方向进行判1，如果为1，面积加1，深度递归，其中主要到的是访问过的1即土地，我们可以将其置为别的数，如-1，这样达到剪枝的目的，然后与之前的最大值比较，max更新答案。对于0的点，直接跳过。

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        ans, row, col = 0, len(grid), len(grid[0])
        
        def neighbors(i, j):
            for nr, nc in ((i, j + 1), (i, j - 1), (i + 1, j), (i - 1, j)):
                if 0 <= nr < row and 0 <= nc < col:
                    yield nr, nc  # 迭代器生成邻居
        
        def dfs(i, j):
            if grid[i][j] == 1:
                temp = 1
                grid[i][j] = -1  # 访问过的1置为-1，剪枝处理
                for new_i, new_j in neighbors(i, j):
                    temp += dfs(new_i, new_j)
                return temp
            else:
                return 0
    
        for i in range(row):
            for j in range(col):
                if grid[i][j] == 1:
                    ans = max(ans, dfs(i, j))
        
        return ans
```

## 最大人工岛（岛屿最大面积的升级版）

在上面题目的基础上，增加一次可以将0换成1 的机会，当然也可以不使用，求最大岛屿面积。

**思路**：基础的处理代码跟上面没有差别，最后多一步将0换成1的处理，然后连接上下左右四个方向，其中注意的是，之前为防止重复计算，每一个单独的岛屿要加一个标志来区分，同时利用字典来存储每一个岛屿的面积。

```python
class Solution:
    def largestIsland(self, grid: List[List[int]]) -> int:
        N = len(grid)

        def neighbors(r, c):
            for nr, nc in ((r-1, c), (r+1, c), (r, c-1), (r, c+1)):
                if 0 <= nr < N and 0 <= nc < N:
                    yield nr, nc

        def dfs(r, c, index):
            ans = 1
            grid[r][c] = index
            for nr, nc in neighbors(r, c):
                if grid[nr][nc] == 1:
                    ans += dfs(nr, nc, index)
            return ans

        area = {}
        index = 2
        for r in range(N):
            for c in range(N):
                if grid[r][c] == 1:
                    area[index] = dfs(r, c, index)
                    index += 1

        ans = max(area.values() or [0])
        for r in range(N):
            for c in range(N):
                if grid[r][c] == 0:
                    seen = {grid[nr][nc] for nr, nc in neighbors(r, c) if grid[nr][nc] > 1}
                    ans = max(ans, 1 + sum(area[i] for i in seen))
        return ans
```

