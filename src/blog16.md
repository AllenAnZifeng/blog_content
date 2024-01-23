# Data Structure

Data: 2023/10/30\



## Matrix

```python
# 初始化矩阵
zero_matrix = [[0 for _ in range(len(matrix[0]))] for _ in range(len(matrix))]
# 注意不能这样 --> 复制了同一个list reference, 会导致修改一个list的值, 其他list也会跟着改变
zero_matrix = [[0] * len(matrix[0])] * len(matrix)

# 矩阵 Transpose
transposed_matrix = zip(*matrix)
# 先对row写逻辑,然后transpose再对col使用
```




## Linked List

- 使用Dummy node, 使得head不需要特殊处理
- 双指针
  - Detecting cycles - Have two pointers, where one pointer increments twice as much as the other, if the two pointers meet, means that there is a cycle
  - Getting the kth from last node - Have two pointers, where one is k nodes ahead of the other. When the node ahead reaches the end, the other node is k nodes behind
  - Getting the middle node - Have two pointers, where one pointer increments twice as much as the other. When the faster node reaches the end of the list, the slower node will be at the middle
