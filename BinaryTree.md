# Binary Tree

[TOC]



#### [2096. Step-By-Step Directions From a Binary Tree Node to Another](https://leetcode.cn/problems/step-by-step-directions-from-a-binary-tree-node-to-another/)

Find the shortest path from $s$ to $t$ consisting of 'L', 'R', and 'U'. All the values in the tree are unique.

```python
class TreeNode:
    def __init__:(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right


# O(N) by recording father
def getDirections(root, startVal, destVal):
    def dfs(node, val):
        '''search for val, record father, return the target node'''
        if node.val == val:
            return node
        if node.left:
            res = dfs(node.left, val)
            if res:
                father[node.left] = node
                return res
        if node.right:
            res = dfs(node.right, val)
            if res:
                father[node.right] = node
                return res
        return False
        
    node1 = dfs(root, startValue)
    node2 = dfs(root, destValue)
    
    def getPath(node):
        '''path from node to root'''
        res = []
        while node != root:
            fa = father[node]
            tmp = 'L' if fa.left == node else 'R'
            res.append(tmp)
            node = fa
        return res
    
    path1 = getPath(node1)
    path2 = getPath(node2)

    # remove common string at the end
    i = len(path1) - 1
    j = len(path2) - 1
    while i >= 0 and j >= 0:
        if path1[i] == path2[j]:
            i -= 1
            j -= 1
        else:
            break
    path2 = path2[: j + 1]    def f(root, startValue, destValue):
    father = {}
    path2.reverse()
    return 'U' * (i + 1) + ''.join(path2)


# method2: O(N) but not good for multi queries
def getDirections(root, startVal, destVal):
    path1 = []
    path2 = []
    def dfs(node, val, path):
        if node.val == val:
            return True
        if node.left:
            res = dfs(node.left, val, path)
            if res:
                path.append('L')
                return True
        if node.right:
            res = dfs(node.right, val, path)
            if res:
                path.append('R')
                return True
        return False
    dfs(root, startValue, path1)
    dfs(root, destValue, path2)
    i, j = len(path1) - 1, len(path2) - 1
    while i >= 0 and j >= 0:
        if path1[i] == path2[j]:
            i -= 1
            j -= 1
        else:
            break
    path2  = path2[: j + 1]
    path2.reverse()
    return 'U' * (i + 1) + ''.join(path2)
```

#### [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

```python
def lowestCommonAncestor(root, p, q):
    def dfs(node, p, q):
        if node is None:
            return False
        
        if node == p or node == q:
            return node

        # all in one side -> return res
        # in two side -> return node
        res_left = dfs(node.left, p, q)
        res_right = dfs(node.right, p, q)
        
        if res_left and res_right:
            return node
        
        return res_left or res_right

    return dfs(root, p, q)
```

