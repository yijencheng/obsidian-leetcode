# 模板
🍎 

```python

```

## 範例1: 102 Binary Tree Level Order Traversal 
題目：[https://leetcode.com/problems/binary-tree-level-order-traversal/description/](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)
敘述：Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).
Ex.
> **Input:** root = `[3,9,20,null,null,15,7]`
   **Output:** `[[3],[9,20],[15,7]]`

思考
* 運用 [[遍歷 Tree – Depth First]] 方法
```python
def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root: return []
    
        ans = defaultdict(list)
        def dfs(level, root):
            if not root: return
            
            ans[level].append(root.val)
            dfs(level+1, root.left)
            dfs(level+1, root.right)
        dfs(0, root)
        
        ## 轉換成答案的format
        final = [[] for i in range(len(ans.keys()))]
        for i, elem in ans.items():
            final[i] = elem
        return final

		## Or Cleaner Version
		def dfs(level, node):
			if not node:
				return
			# Create a new level if needed
			if len(ans) == level:
				ans.append([])
	
			ans[level].append(node.val)
			dfs(level + 1, node.left)
			dfs(level + 1, node.right)
	
		dfs(0, root)
		return ans
```

## 範例2: 104. Maximum Depth of Binary Tree
題目：[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
敘述：Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.
Ex.
> **Input:** root = [3,9,20,null,null,15,7]
   **Output:** 3

```python
	def maxDepth(self, root: Optional[TreeNode]) -> int:
        d = [0]
        def dfs(level, cur):
            if not cur:
                return
            d[0] = max(d[0], level+1) ## update every time meet new item
            dfs(level+1, cur.left)
            dfs(level+1, cur.right)
        dfs(0, root)
        return d[0]
```