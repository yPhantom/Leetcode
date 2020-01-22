## 数组

记录一些数组题目的解法

### 1.Microsoft | OA 2019 | Numbers With Equal Digit Sum

![](https://assets.leetcode.com/users/siojl13/image_1570305047.png)

[原题链接](https://leetcode.com/discuss/interview-question/365872/)

```python
求任意两个位上的数字相加和相等的两位数的最大和，Two-sum 变体题，注意：

def find_digit_sum(num):
	val = 0 
	while num:
		val += num % 10 
		num //= 10 

	return val 

def num_digit_equal_sum(arr):
	digit_sum_map = {} 
	max_val = -1 
	for num in arr:
		digit_sum = find_digit_sum(num) 
		if digit_sum in digit_sum_map:
			other_val = digit_sum_map[digit_sum] 
			max_val = max(max_val, other_val + num) 
			digit_sum_map[digit_sum] = max(other_val, num) 
		else:
			digit_sum_map[digit_sum] = num

	return max_val 
```

