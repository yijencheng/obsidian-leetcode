
# 摘要
Tree 是Array的進階版。有別於 Array 是線性結構，[[遍歷 Array]] 時只需要用一個 for-loop，Tree Traversal 要更複雜，通常會使用到『遞迴 (recursion) 』技巧。

因此這章節會介紹 tree traversal 三種方式— inOrder/preOrder/postOrder。並且這也是recursion 的初步學習的第一步。


## 概念解釋
首先先思考 Array 遍歷：
```python
for i, cur in enumerate(arr):
	print(f"index:{i}, cur:{cur}")
```
可以發現有兩個關鍵值：1) cur代表現在的「值」2) index 代表「位置資訊」。
在 Array 這種線性結構中，位置很好理解，代表第i個元素的順位。但在 tree 的結構中，要如何表達元素的位置呢？
答案是 level （or depth)  。

```python
def dfs(level, cur):
	if not cur:
		return

	// do sth preOrder
	dfs(level+1, cur.left)
	// do sth inOrder
	dfs(level+1, cur.right)
	// do sth postOrder

dfs(0, root)
```
上面可以發現 level, cur 對應了Array 的 i,cur。 