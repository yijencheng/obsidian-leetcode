Leetcode 314 Premium
![[Pasted image 20250806131512.png]]

思路:
思考Tree 結構的時候, 通常解法離不開 dfs 或是 bfs 的變形, 這題是希望回傳一個垂直的list按照左至右回傳, 這樣的解法通常離不開三種樹結構很常做的考法, 如果考慮用dfs 大多是preorder, postorder, inorder三種做法來調整, 如果考慮用 bfs的話是另外一個思路, 那我們一個一個來討論

首先是先討論 Preorder, Postorder 和 inorder
#### DFS
三種方向 Recap
inorder (左上右)
Preorder (上左右)
postorder (左右上)

先寫出DFS常見模板

```python
def Solution(root):
	def dfs(root):
		if not root:
			return
		dfs(root.left)
		dfs(root.right)
	return
```


再來是要搜集node並且回傳, 想像上我會訂一個 index, 去規劃, 往左邊再深入 - 1, 往右邊 + 1, 來確保我的node是都在同一個垂直域上, 因此改編模板成下面

```python
def Solution(root):
	def dfs(root, index):
		if not root:
			return
		dfs(root.left, index - 1)
		dfs(root.right, index + 1)
	dfs(root, 0)
	return
```

那到底我要用哪一種遍歷方式才會在每個list內得到正確的順序呢

```python
def Solution(root):
	abc = defaultdict(list)
	def dfs(root, index):
		if not root:
			return
		# abc[index].append(root.val) # Preorder
		dfs(root.left, index - 1)
		# abc[index].append(root.val) # inorder
		dfs(root.right, index + 1)
		# abc[index].append(root.val) # Postorder
		return
	dfs(root, 0)

```

上面顯示了不同 order, list應該擺放的位置, 看起來是 Preorder 比較適合

```python
def Solution(root):
	abc = defaultdict(list)
	def dfs(root, index):
		if not root:
			return
		abc[index].append(root.val) # Preorder
		dfs(root.left, index - 1)
		# abc[index].append(root.val) # inorder
		dfs(root.right, index + 1)
		# abc[index].append(root.val) # Postorder
		return
	dfs(root, 0)
	keys = list(abc.keys())
	keys.sort()
	result = []
	for k in keys:
		result.append(abc[k])
	return result
```

但其實這也不是對的, 結論還是先定義level後 sort一遍才會把所有case解決

```python
def Solution(root):
	abc = defaultdict(list)
	def dfs(root, index, level):
		if not root:
			return
		abc[index].append((root.val,level))
		dfs(root.left, index -1, level + 1)
		dfs(root.right, index -1, level + 1)
		return
	dfs(root, 0, 0)
	keys = list(abc.keys())
	keys.sort()
	result = []
	for k in keys:
		abc[k].sort(key=lambda x: x[1])
		result.append([ x[0] for x in abc[k]])
	return result
```

因此我們得到答案是對的
