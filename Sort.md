# Sort

#### [215. Kth Largest Element in an Array](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

Time: Exp $O(N)$

Space: Exp $O(\log N)$

```python
def findKthLargest(arr, k):
    def swap(arr, i, j):
        tmp = arr[i]
        arr[i] = arr[j]
        arr[j] = tmp
        
    def partition(arr, l, r):
        i = l - 1
        random_idx = (l + r) >> 1
        x = arr[random_idx]
        swap(arr, r, random_idx)
        for j in range(r):
            if arr[j] >= x:
                i += 1
                swap(arr, i, j)
        swap(arr, i + 1, r)
        return i + 1, x
        
    l, r = 0, len(arr) - 1
    while True:
        idx, x = partition(arr, l, r)
        if idx == k:
            return x
        if idx > k:
            l = idx - 1
        else:
            r = idx + 1

```

