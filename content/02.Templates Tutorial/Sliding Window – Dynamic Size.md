編輯: @yijencheng
# 摘要
Sliding Window 是一個演算法，通常是針對[[找出Array中所有符合條件的最佳解]]進行的優化。
<iframe width="560" height="315" src="https://www.youtube.com/embed/GcW4mgmgSbw?si=ZTYGQGikCAgV818X" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


# 模板

🍎 參數初始化
* 選擇 window資料結構：常用 [[Dict (HashMap)]]
* Left/start 指針: 在縮小window時，用來追蹤window左邊界
* ans：用來存最佳答案
🍎 [[遍歷 array]] 
* 用for/while loop遍歷
🍎 縮小window，找到包含新元素的答案
* 決定先將新元素加入window（多數）還是後加
* **判斷window限制，用while loop縮小窗口**
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

## 判斷window限制，用while-loop縮小窗口

# 例題
### 範例1
題目：[Maximum sum of subarray with size k](https://www.geeksforgeeks.org/dsa/find-maximum-minimum-sum-subarray-size-k/)
敘述：Given an array of integers and a number k, find the maximum sum of a subarray of size k.
Example 1: 
> Input  : arr = {100, 200, 300, 400},  k = 2
> Output : 700

思考：
```python
window = []
left = 0
ans = 0
for i, num in enumerate(arr):
	## 縮小window，找到包含新元素的答案
	window.append(num) 
	while len(window) > k:
		window.pop(0) ## pop操作很慢
		left+=1
	ans = max(ans, sum(window)) ## sum操作很慢
return ans
```
```python
## queue 優化版
q = deque()
left = 0
ans = 0
for i, num in enumerate(arr):
	## 縮小window，找到包含新元素的答案
	q.append(num) 
	while len(q) > k:
		q.popleft() ## pop操作O(1)
		left+=1
	## 更新答案
	ans = max(ans, sum(q)) ## sum操作還是慢
```
註解：這題實際上有更佳解法，但這裡想透過這個比較簡單的題目，介紹動態窗口的寫法。

### 範例2
題目：[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
敘述：Given a string s, find the length of the longest substring without duplicate characters.
思考：
* 首先注意到“without duplicate characters“，第一直覺會想用 set()。但在sliding window–dynamic size題型中，調整window大小時需要紀錄左邊移動到哪個index，set()無法滿足
* 接著思考如何判斷 “duplicate characters" ，可以先聯想到用 dict 這個結構。接著有兩種判斷方式 1) 如果是先加入新的元素且發生重複，則 dict[key] > 1 2) 如果要保證加入新元素不會重複，則要看 key in d
```python
# 先加入新元素
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


# 最後加入新元素
def lengthOfLongestSubstring(s: str) -> int:
	## 參數初始化
	d = {}
	l = 0
	longest = 0
	
	## 遍歷
	for i, ch in enumerate(s):
		## 縮小window，確保加入新元素後會是符合條件的答案
		while ch in d:
			d[s[l]] -=1
			if d[s[l]] == 0:del d[s[l]]
			l+=1
		d[ch] = d.get(ch, 0)+1
		## 更新答案
		longest = max(i-l+1, longest)
	return longest
```

### 範例3
題目：[904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/description/)
概念：Longest Substring with K Distinct Characters
```python
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
			if d[fruits[left]] == 0: del d[fruits[left]] 
			left +=1
		ans = max(ans, i-left+1)
	return ans
```
註解：
* 字典的Key數量會影響上面while判斷，因此需要移除（保證不超過兩個）
# 討論
在上面範例中：範例1 對window的限制是「長度為k」、範例 2, 3 的限制則是「最多 K Distinct Characters」。其他類似題型會有不同窗口限制，因此須熟悉各種資料結構的特性：

1. Maximum/Minimum Subarray Sum:
2. Longest Substring with K Distinct Characters:
3. Longest Subarray with Ones after Replacement:
4. Find All Anagrams in a String:
5. Smallest Subarray with Sum at Least K:
6. Maximum Consecutive Ones after Flipping Zeros:
7. Minimum Window Substring:
8. Longest Repeating Character Replacement:
9. Fruit Into Baskets:
10. Subarrays with Product Less than K

| Problem                                                                                                                          | 窗口限制               | 資料結構 |
| -------------------------------------------------------------------------------------------------------------------------------- | ------------------ | ---- |
| [Maximum Subarray Sum](https://leetcode.com/problems/maximum-subarray)                                                           |                    |      |
| [Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k) | Distinct, Lenght=K |      |
|                                                                                                                                  |                    |      |
