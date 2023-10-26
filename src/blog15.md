# Recursion

Data: 2023/10/26\



## [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/submissions/1084870339/)
给了一个tree和targetSum, 返回从root到leafnode可以加到targetSum的路径

解法一(top-down recursion)
空间复杂度高
```python
class Solution:
    def pathSum(self, root , targetSum: int) -> List[List[int]]:

        res = []
        def recur(root,targetSum,path):
            if root == None:
                return None

            path.append(root.val)

            if root.val == targetSum and root.left == None and root.right == None:
                res.append(path)

            recur(root.left,targetSum-root.val,path[:])
            recur(root.right, targetSum-root.val, path[:])


        recur(root,targetSum,[])

        return res
```

解法二(backtracking recursion)
相比解法一节约空间
用一个var记录当前的状态current path, 而不是用stack里的path copy记录状态

```python
class Solution:
    def pathSum(self, root , targetSum: int) -> List[List[int]]:
        res = []
        cur = []

        def recur(root,targetSum):
            if root == None:
                return None

            cur.append(root.val)
            if root.val == targetSum and root.left == None and root.right == None:
                res.append(cur[:])

            recur(root.left,targetSum-root.val)
            recur(root.right,targetSum-root.val)
            cur.pop()

        recur(root,targetSum)
        return res
```

解法三(bottom-up recursion)
相比解法一节约空间
```python
class Solution:
    def pathSum(self, root , targetSum: int) -> List[List[int]]:

        def recur(root,targetSum):
            if root == None:
                return []

            if root.val == targetSum and root.left == None and root.right == None:
                return [[root.val]]

            left = recur(root.left,targetSum-root.val)
            right = recur(root.right, targetSum - root.val)

            validPaths = left + right # list concatenation

            return [[root.val] + p for p in validPaths]

        return recur(root,targetSum)
```

