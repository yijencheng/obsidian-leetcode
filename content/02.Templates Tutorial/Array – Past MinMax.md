
# 摘要
這個模板，屬於 [[Array – Past Exist]] 模板更通用/進階版本。
# 模板
🍎 “假設”你是先知 (i.e 知道過去的所有資訊)，當遇到新元素$arr[i]$時，該如何計算答案
🍎 每次for-loop最後，加上「更新歷史資訊」的邏輯、並決定變數集資料結構

```
d = {} //用來
for i,num in enumerate(nums):
	// 根據手上歷史的 mix/max/... 值，更新答案（也可以因為根據題目要求，符合/不符合條件而提前結束) 

	// 更新涵蓋新元素後，新的 mix/max/... 值
```



## 範例1: Best Time to Buy and Sell Stock
題目：[https://leetcode.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
思考
* 如果可以買更低、為什麼要買高？ 
* 每個新元素i出現時，都能作為更高的賣出點。但必須等這輪結束後，才能成為下一個 i+1元素的買入點

```python
def maxProfit(self, prices: List[int]) -> int:
	ans = 0
	lowest_buy = prices[0] # 也可以是 inf
	for p in prices:
		ans = max(ans, p-lowest_buy)
		
		lowest_buy = min(lowest_buy, p) # 更新最低買入
	return ans
```

## 範例2: Jump Game

思考
* 
``` python
def canJump(self, nums: List[int]) -> bool:
	farthest = 0

	for i in range(len(nums)):
		if farthest < i:
			return False

		farthest = max(farthest, i + nums[i]) # 更新最遠可以走到的距離
	return farthest >= len(nums)-1
```