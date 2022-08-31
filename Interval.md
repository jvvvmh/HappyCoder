# Interval

#### [729. My Calendar I](https://leetcode.cn/problems/my-calendar-i/)

方法一: sorted dict      time $O(N\log N)$   space $O(N)$

方法二: segment tree  time $O(N\log C)$   $O(N\log C)$  $C$ is the max tree depth (10^6) 

```c++
class MyCalendar {
public:
    map<int, int> mp; // set<pair<int,int>>

    bool book(int start, int end) {
        auto it = mp.lower_bound(end);

        if  (it == mp.end()){
            if (mp.size() && (--it)->second > start){
                return false;
            }
            mp[start] = end;
            return true;
        }

        if (it == mp.begin()){
            if (it->first >= end){
                mp[start]=end;
                return true;
            }
            return false;
        }

        if (it->first >= end && (--it)->second <= start){
            mp[start] = end;
            return true;
        }
        return false;
    }
};
```

```python
class Node(object):
    def __init__(self, l, r):
        self.l = l
        self.r = r
        self.mid = (l + r) // 2
        self.cnt = 0
        self.lc = None
        self.rc = None

def updateDown(node):
    node.cnt = node.r - node.l + 1
    if node.lc: updateDown(node.lc)
    if node.lc: updateDown(node.rc)

def dfs(node, st, ed, write):
    l, mid, r = node.l, node.mid, node.r
    if l == st and r == ed:
        if node.cnt > 0:
            return False
        else:
            if write: updateDown(node)
            return True
    
    # [l, mid] [mid + 1, r]
    
    if node.lc == None and node.rc == None:
        node.lc = Node(l, mid)
        node.rc = Node(mid + 1, r)
        if node.cnt == (r - l + 1):
            node.lc.cnt = (node.lc.r - node.lc.l + 1)
            node.rc.cnt = (node.rc.r - node.rc.l + 1)

    if ed <= mid:
        res = dfs(node.lc, st, ed, write)
        if res:
            if write:
                node.cnt = node.lc.cnt + (node.rc.cnt if node.rc else 0)
            return True
        return False

    if st >= mid + 1:
        res = dfs(node.rc, st, ed, write)
        if res:
            if write:
                node.cnt = node.rc.cnt + (node.lc.cnt if node.lc else 0)
            return True
        return False
    
    if (dfs(node.lc, st, mid, write) and dfs(node.rc, mid + 1, ed, write)):
        if write:
            node.cnt = node.lc.cnt + node.rc.cnt
        return True

    return False


class MyCalendar(object):

    def __init__(self):
        self.root = Node(0, 10**9)

    def book(self, start, end):
        if dfs(self.root, start, end-1, False):
            dfs(self.root, start, end-1, True)
            return True
        return False
```



Segment Tree:

#### [2158. Amount of New Area Painted Each Day](https://leetcode.cn/problems/amount-of-new-area-painted-each-day/)





[715. Range 模块](https://leetcode.cn/problems/range-module/)、[2276. 统计区间中的整数数目](https://leetcode.cn/problems/count-integers-in-intervals/)

