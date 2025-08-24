# 模板

🍎 選儲存結構
* 如果只是紀錄值，可以用sets。如果需要紀錄 value, index，可以用 dict
* 
```
d = {} //用來紀錄過去出現的元素
for i,num in enumerate(nums):
	// 確認符合條件，決定是否return 
	
	// 將新元素加入紀錄
```

## 範例1: Contains Duplicate
```
def containsDuplicate(self, nums: List[int]) -> bool:
	s = set()
	for num in nums:
		if num in s:
			return True
		s.add(num)
	return False
```
## 範例2: Two sum
題目：[https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/)
思考
* 不只要找到「過去某個元素是否存在」，還要找到他對應的位置 (index)
* 因此要使用dictionary
```
def twoSum(self, nums: List[int], target: int) -> List[int]:
	d = {}
	for i,num in enumerate(nums):
		match = target-num
		if match in d:
			return [d[match], i]
		d[num] = i
```

