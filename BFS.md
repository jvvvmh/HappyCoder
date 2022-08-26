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

