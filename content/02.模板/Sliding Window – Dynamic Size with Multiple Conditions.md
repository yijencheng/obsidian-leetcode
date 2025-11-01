Prev: [[Sliding Window – Dynamic Size]]
# 模板
```python
## 參數初始化
d = {}
left = 0 
ans = 0
## 遍歷
for i, num in enumerate(nums):
	## 縮小window，找到包含新元素的答案
	d[num] = d.get(num,0)+1
	while {condition_1, condition_2...}:
		d[num[i]] -=1
		left +=1

	## 更新答案
	if {not condition_i}:
		ans = max(ans, i-left+1)
```
# 例題
## 範例1

題目：[2106. Maximum Fruits Harvested After at Most K Steps](https://leetcode.com/problems/maximum-fruits-harvested-after-at-most-k-steps/)
敘述：Fruits are available at some positions on an infinite x-axis. You are given a 2D integer array `fruits` where `fruits[i] = [positioni, amounti]` depicts `amounti` fruits at the position `positioni`. `fruits` is already **sorted** by `positioni` in **ascending order**, and each `positioni` is **unique**.

You are also given an integer `startPos` and an integer `k`. Initially, you are at the position `startPos`. From any position, you can either walk to the **left or right**. It takes **one step** to move **one unit** on the x-axis, and you can walk **at most** `k` steps in total. For every position you reach, you harvest all the fruits at that position, and the fruits will disappear from that position.

Return _the **maximum total number** of fruits you can harvest_.

Example 1:
> Input: fruits = [ [2,8],[6,3],[8,6] ], startPos = 5, k = 4
> Output: 9
> Explanation: 
> The optimal way is to:Move right to position 6 and harvest 3 fruits
> - Move right to position 8 and harvest 6 fruits
> - You moved 3 steps and harvested 3 + 6 = 9 fruits in total.

思考：
* 首先，發現答案可能有四種：全部向右走、全部向左走、先左再右跟先右再左
* 發現前兩個可以輕鬆處理
* 發現先左再右（跟先右再左）本質上是一個動態窗口問題，並且這兩個可以合併進行 >> 這是本題關鍵
* 決定左右邊界、以及窗口的限制條件
解法：

```python
def maxTotalFruits(self, fruits: List[List[int]], startPos: int, k: int) -> int:
        left_ans, right_ans = 0,0
        for fruit in fruits:
            if fruit[0]>=startPos-k and fruit[0]<=startPos:
	            left_ans+=fruit[1]
            if fruit[0]>=startPos and fruit[0]<=startPos+k:
	            right_ans+=fruit[1]
        ans = max(left_ans, right_ans)

        def min_step(left, right):
            return (
				min(
					abs(startPos - fruits[right][0]),
					abs(startPos - fruits[left][0]),
				)
				+ fruits[right][0]
				- fruits[left][0]
			)
	    # 主要邏輯
        left = 0
        total = 0
        for right in range(len(fruits)):
            total += fruits[right][1]
            if fruits[right][0]<=startPos:
	            continue
            while fruits[left][0]<startPos and min_step(left,right)> k:
                total-=fruits[left][1]
                left+=1
            if min_step(left,right)<= k: #重要
                ans = max(ans, total)
        return ans
```