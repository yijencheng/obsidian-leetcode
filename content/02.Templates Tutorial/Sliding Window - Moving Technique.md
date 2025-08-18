[3652. Best Time to Buy and Sell Stock using Strategy](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-using-strategy/)

題意:
給你每日的股票價格, 跟每日的股票策略, 策略有三種, -1代表買入, 0代表持有, 1代表賣出
你可以有至多一種改動, 改動有條件如下, 
	你只能選給定的k 長度連續sub array, 
	前 k//2 的策略變成持有, 
	後 k//2 的策略變成賣出.
求至多一種改動後內的最大收益, 收益定義為 `prices[i] * strategy[i]`

想法:
 他寫長度為 k 的 連續 sub array, 想到 sliding window也是這個特性, 
 再來是如何運用這個特性來解題 --> 求出最大收益的話 我是不是只要把兩種case處理出來即可
 第一種 不給改動的原收益
 第二種 有給一個改動的收益 (所有可能都算過一次)
 找出 max(第一種, 第二種)

第一種我們可以直接先算出來
至於第二種就是今天要討論的sliding window - moving technique

```python
class Solution:
	def sumofkarray(self, array, k)

# 想法
arrayA
n = len(arrayA)
window_size = k
tmp_result = 0
for i in range(k):
	tmp_result += arrayA[i]
	{舉例: 我想要的第一版sliding window初始的計算}

# 移動你的sliding window
for j in range(n - k)
	tmp_result -= arrayA[j-1]
	tmp_result += arrayA[j+k]
	{更新你的sliding window想要算的數值, 要注意邊界條件的更新}
	{比方說上方的例子就是 減去前一個index的array數值, 加上下一個index的array數值(因為你在做sliding window)}
	{更新完是否需要比較答案}
	xxx_max_result = max(tmp_result, xxx_max_result)
return xxx_max_result
	


```

延伸回今天這題有兩個關鍵:
第一個算出起始的sliding window解法, 
 以題目的敘述為例:  你會需要作出以下的處理
* 把此index到 index + k//2 前的股票歸零
* 把此index + k//2 到 index + k 的策略數值從原始策略都改成賣出
要做到以上兩點有個共通點是要做的就是 
* 先減去原始在k array上的收益, 
* 再補上改動完的收益增值

第二個想出邊界條件的更新
主要是邊界條件的更新比較難
想法: 當今天由左至右shift index一格window時, 有以下三個邊界條件變動
* 還沒shift前最左邊的 index 的原始收益需要被加回來
* shift完中間的 index 需要被歸零
* shift完最右邊的 index 原始受益先被扣除 再加上賣出策略的收益


```python
class Solution:
	def maxProfit(self, prices: List[int], strategy: List[int], k: int) -> int:
		max_profits = -inf
		tmp_profits = 0
		
		n = len(prices)
		# 第一種: 不給改動的原收益
		for i in range(n):
			tmp_profits += prices[i] * strategy[i]
			max_profits = max(max_profits, tmp_profits)
		window_size = k
		
		# init first k
		# 第二種: 初始化 當今天我的改動起始點是index 0的時候算出來的收益
		for j in range(k):
			
			tmp_profits -= prices[j] * strategy[j]
		
			if j >= k // 2:
				tmp_profits += prices[j]
				max_profits = max(tmp_profits, max_profits)

		# iterate to the end
		for i in range(n - k):
			# fill up the first index
			tmp_profits += prices[i] * strategy[i]
			# minus half index and minus original end index and add-on end index
			tmp_profits -= prices[i + k // 2]
			tmp_profits -= prices[i+k] * strategy[i+k]
			tmp_profits += prices[i+k]
			max_profits = max(tmp_profits, max_profits)
			
			return max_profits

```