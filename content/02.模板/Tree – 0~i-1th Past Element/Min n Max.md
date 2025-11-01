
# 摘要
這個模板屬於 [[1.Check Exist]] 的通用/進階版本。
# 模板
🍎 “假設”你是先知 (i.e 知道過去所有資訊)
🍎 當遇到新元素$arr[i]$時，該如何計算答案
🍎 每次for-loop最後，加上「更新歷史資訊」的邏輯、並決定變數集資料結構

```python
d = {} //用來
for i,num in enumerate(nums):
	// 根據手上歷史的 mix/max/... 值，更新答案（也可以因為根據題目要求，符合/不符合條件而提前結束) 

	// 更新涵蓋新元素後，新的 mix/max/... 值
```


## 範例2: 104. Maximum Depth of Binary Tree
題目：[https://leetcode.com/problems/count-good-nodes-in-binary-tree/](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)
敘述：

```python
	def goodNodes(self, root: TreeNode) -> int:
        goodNode = [0]
        def dfs(curMax, root):
            if not root:return
            if curMax <= root.val:
                goodNode[0]+=1
                curMax = root.val
            dfs(curMax, root.left)
            dfs(curMax, root.right)
        
        dfs(root.val, root)
        return goodNode[0]
