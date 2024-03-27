# Dijkstra

Data: 2024/03/26\

### [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/)

```python
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        # 首先创建一个graph
        # distance 是用来记录从k到每个node的当前最短距离
        distance = {}
        g = {}
        for i in range(1,n+1):
            distance[i] = float('inf') # 初始化每个node到k的距离是无穷大
            g[i] = {}
        distance[k] = 0 # 从k到k的距离是0
        
        for src,dst,weight in times:
            g[src][dst] = weight  # directed graph
        
        h = [(0,k)] # min heap (distance to k, node)

        while h:
            dist, cur = heapq.heappop(h) # dist: 从k到cur的距离
            for nei,weight in g[cur].items(): # 遍历cur的邻居们, weight: 从cur到nei的距离
                if dist + weight < distance[nei]: 
                    distance[nei] = dist + weight
                    heapq.heappush(h,(distance[nei],nei))
        
        res = max(distance.values())
        if res == float('inf'):
            return -1
        return res
```

注意: 
- heap中会出现重复访问的node, 但是min heap确保最短距离的node会被pop出来.
- `dist + weight < distance[nei]` 这个更新条件确保重复访问并且不合格的node不会被再加入heap
- distance字典起到了visit set的作用
- 这个问题不能用visit set因为有些node就是会被重复访问, 如果能够更新最短距离,就需要再次更新他所有的邻居


