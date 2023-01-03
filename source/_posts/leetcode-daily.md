---
title: leetcode 刷题笔记
date: 2021-08-16 19:42:44
tags:
- 技术
- leetcode
categories: leetcode
description: 偶尔想起的刷题
photos:
---
# 环境信息

* 原帖链接：[leetcode-pdf](https://github.com/doocs/leetcode/commit/59d54a57a62e21235fe25a5b7b72d8136469971e)
* git 管理
* ide：vscode
* 开发语言：java + python

# 那些还未掌握的思路

[深度优先搜索+路径记录](https://github.com/doocs/leetcode/blob/main/solution/0100-0199/0113.Path%20Sum%20II/README.md)

深度优先搜索，就是一种遍历的方式，优先使用递归的写法，可以让自己更专注处理好对遍历当前节点处理时候，更为集中精力。

这道例题很好的说明处理遍历节点的骚操作，就是专注眼前，化繁为简（二叉树的每个节点事同质的，所以专注处理一个节点就可以，节点的串联关系交给遍历就可以）

```python
def dfs(root, sum):
    if root is None:
      return
    path.append(root.val) #处理当前节点
    if root.val == sum and root.left is None and root.right is None:
      res.append(path.copy()) # 终止条件
      dfs(root.left, sum - root.val)
      dfs(root.right, sum - root.val)
      path.pop() # 骚操作
```

