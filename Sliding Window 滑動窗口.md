編輯: @yijencheng
# 摘要
Sliding Window 是一個演算法，通常是針對[[找出Array中所有符合條件的最佳解]]進行的優化。
<iframe width="560" height="315" src="https://www.youtube.com/embed/GcW4mgmgSbw?si=ZTYGQGikCAgV818X" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


# 模板
🍎 參數初始化
* 選擇 window資料結構：如果是Fix-Sized多用Array、動態長度則常見使用 [[Dict (HashMap)]]
* Left/start 指針: 在縮小window時，用來追蹤window的左邊界
* ans 變數最佳答案

🍎 [[遍歷 array]] 
* 用for/while loop遍歷
🍎 縮小window，找到包含新元素的答案
* 決定先將新元素加入window（多數），還是後加
* 建議用while loop 
🍎 更新答案
```python
## 參數初始化
d = {}
left = 0 
ans = 0
## 遍歷
for i, num in enumerate(nums):
	## 縮小window，找到包含新元素的答案
	d[num] = d.get(num,0)+1
	while d[num] > 1:
		d[num[i]] -=1
		left +=1

	## 更新答案
	ans = max(ans, i-left+1)
	
```

# 範例2
題目：[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
```run-python
def lengthOfLongestSubstring(s: str) -> int:
	## 參數初始化
	d = {}
	l = 0
	longest = 0
	
	## 遍歷
	for i, ch in enumerate(s):
		## 縮小window，找到包含新元素的答案
		d[ch] = d.get(ch, 0)+1
		while d[ch]>1:
			d[s[l]] -=1
			l+=1
		## 更新答案
		longest = max(i-l+1, longest)
	return longest


if __name__ == "__main__":
	ans = lengthOfLongestSubstring("abcabcbb") 
	print(ans) # Output: 3

```

# 範例3
題目：[904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/description/)
```run-python
def totalFruit(self, fruits: List[int]) -> int:
	d = {}
	left = 0
	ans = 0
	## 遍歷
	for i, fruit in enumerate(fruits):
		## 縮小window，找到包含新元素的答案
		d[fruit] = d.get(fruit,0)+1
		while len(d) >2:
			d[fruits[left]] -=1
			## 字典的Key數量會影響上面while判斷，因此需要移除（保證不超過兩個）
			if d[fruits[left]] == 0: del d[fruits[left]] 
			left +=1
		ans = max(ans, i-left+1)
	return ans
```


# 觀察
