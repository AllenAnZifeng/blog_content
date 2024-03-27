# MST

Data: 2024/03/26\

### [1135. Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)


## Prim's Algorithm
对node进行贪婪
```python
class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        points = [tuple(point) for point in points]
        g = {}
        for i in range(len(points)):
            g[points[i]] = {}

        for i in range(len(points)):   # 创建双向全连接图
            for j in range(i+1,len(points)):
                xi,yi = points[i]
                xj,yj = points[j]
                g[(xi,yi)][(xj,yj)] = abs(xi-xj) + abs(yi-yj)
                g[(xj,yj)][(xi,yi)] = abs(xi-xj) + abs(yi-yj)

        cost = 0
        h = [] # min heap
        for nei, dist in g[points[0]].items():
            h.append((dist,nei))
        
        heapq.heapify(h)
        visit = set()
        visit.add(points[0])

        while h:
            dist, cur = heapq.heappop(h) # cur是离目前访问过所有node最近的node
            if cur in visit: # 因为每次都是贪婪最短距离,之后的重复访问的node一定会更远,直接跳过
                continue
            visit.add(cur)
            cost += dist
            for nei, weight in g[cur].items():
                heapq.heappush(h,(weight,nei))
        
        return cost
```
注意:
- time complexity: O(n^2 * log(n))
- heap size O(n^2) with heap operation O(log(n))
- 一开始随便从一个node开始,用visit set保证每个node只被访问一次
- 每次都会把新加入的node的所有邻居加入heap
- min heap中会有重复的node, 但是只有离**目前访问过所有node**最近的node会被pop出来

## Kruskal's Algorithm
对edge进行贪婪
```python
class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        points = [tuple(point) for point in points]
        edges = [] # length, n1, n2
        for i in range(len(points)):
            for j in range(i+1,len(points)):
                xi,yi = points[i]
                xj,yj = points[j]
                length = abs(xi-xj) + abs(yi-yj)
                edges.append((length,i,j))
        # print(edges)
        edges.sort(key=lambda x:x[0]) 
  
        dads = {}
        for i in range(len(points)): # 一开始每个node都是自己的大爹
            dads[i] = i

        def find(node): # return parent of node 向上找大爹, 大爹是root node
            while node != dads[node]:
                node = dads[node]
            return node
        
        def union(n1,n2)->bool: # false if already same parent
            if find(n1) == find(n2):
                return False
            dads[find(n1)] = find(n2) # 把n1的大爹的大爹指向n2的大爹
            return True
        
        cost = 0
        for length, n1, n2 in edges:
            if union(n1,n2):
                cost += length
        return cost
```

注意:
- time complexity: O(n^2 * log(n)) --> edge数量是n^2, sort edge是最费时间的
- 一个edge两端的node不能是同一个大爹,否则会形成cycle
- 大爹指的是root node
- union-find 算法来找大爹,合并大爹
- 当合并n1, n2时, 要把n1的大爹的大爹指向n2的大爹,这样n1的所有爹们都指向n2的大爹