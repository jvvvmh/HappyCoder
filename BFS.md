# BFS

#### [818. Race Car](https://leetcode.cn/problems/race-car/)

初始pos=0, speed=1

(1) 加速 pos+=speed speed*=2

(2) 反转 speed = -1 or 1 pos=pos

求到target的最短路径

分析: 最快速序列 (0, 1) -> (1, 2) -> (3, 4) -> (7, 8) ... 对于某个位置$N$, 在那个位置上的速度最多有$\log (N)$种, 所以时间复杂度和空间复杂度都是$N \log N$. 剪枝: pos<0, 可以先正着走再reverse; pos>2^k, 如果走过头再回来，速度一样大，却还多走了。

2^{k-1}  target  2^k

​                           2^k              2*target             2^(k+1)

0  5 8     16

​       2^k  2^(k+1)

```python
def racecar(self, target):
    # 1 pos小于0 或者 > 2*k
    # 记录 d[(pos, v)] 的最优step

    from collections import deque
    q = deque()
    q.append((0, 1, 0)) # pos, v, step
    d = set() # pos, v的set, 如果前面到过了这个状态, 前面的step肯定小, 后面就不做了
    d.add((0, 1))

    def checkValid(pos, v):
        if pos < 0 or (pos > (1 << (target.bit_length() + 1))):
            return False
        if (pos, v) in d: return False
        return True

    step = 0
    while q:
        (pos, v, step) = q.popleft()
        if pos == target:
            return step    

        step2 = step + 1

        # 加速
        pos2 = pos + v
        v2 = v * 2
        if checkValid(pos2, v2):
            q.append((pos2, v2, step2))
            d.add((pos2, v2))
        # 减速
        pos2 = pos
        v2 = 1 if v < 0 else -1
        if checkValid(pos2, v2):
            q.append((pos2, v2, step2))
            d.add((pos2, v2))
    return step
```



#### [1293. Shortest Path in a Grid with Obstacles Elimination](https://leetcode.cn/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

求从左上角到右下角的最短路径，消除obstacles的个数不超过$k$

思路: 剪枝 visited[(x, y)] = cnt (剩余消除路障的额度), 更新只存在于new_cnt > cnt

状态数: $M \times N \times ( \min(k, M+N) )$

```python
def shortestPath(self, grid, k):

    # in deque, (i, j, step, cnt)
    m, n = len(grid), len(grid[0])
    k = min(k, m + n)
    q = deque()
    q.append( (0, 0, 0, k) )

    visited = {}

    while q:
        x, y, step, cnt = q.popleft()
        if x == m - 1 and y == n - 1: return step
        if (x, y) in visited and visited[(x, y)] >= cnt: continue

        visited[(x, y)] = cnt

        for dx, dy in [(1, 0), (-1, 0), (0, 1), (0, -1)]:
            xx = x + dx
            yy = y + dy
            if (0 <= xx and xx < m and 0 <= yy and yy < n):
                if xx == m - 1 and yy == n - 1:
                    return step + 1
                if grid[xx][yy]:
                    if cnt > 0:
                        if (xx, yy) not in visited:
                            q.append( (xx, yy, step+1, cnt-1) )
                        else:
                            if cnt - 1 > visited[(xx, yy)]:
                                q.append( (xx, yy, step+1, cnt-1) )
                else:
                    if (xx, yy) not in visited:
                        q.append( (xx, yy, step+1, cnt) )
                    else:
                        if cnt > visited[(xx, yy)]:
                            q.append( (xx, yy, step+1, cnt) )

    return -1
```

