# LeetCode 题解 - 数组（3）
- [LeetCode 题解 - 数组（3）](#leetcode-题解---数组3)
  - [困难](#困难)
    - [42. 接雨水](#42-接雨水)
  - [非力扣题](#非力扣题)
    - [数组扁平化](#数组扁平化)

## 困难

### 42. 接雨水

```
//暴力，272ms
//找当前位置左右的高度最大值，其中较小的那个减去当前高度就是雨水量
var trap = function(height) {
    let ans = 0;
    for (let i = 0; i<height.length; i++) {
        let leftMax = 0, rightMax = 0;
        for (let j=i; j>=0; j--) {
            leftMax = Math.max(leftMax,height[j])
        }
        for (let j=i; j<height.length; j++) {
            rightMax = Math.max(rightMax, height[j])
        }
        ans += Math.min(leftMax, rightMax) - height[i];
    }
    return ans;
};

//双指针，76ms
var trap = function(height) {
    let ans = 0;
    let left = 0, right = height.length - 1;
    let leftMax = 0, rightMax = 0;
    while (left < right) {
        leftMax = Math.max(leftMax, height[left]);
        rightMax = Math.max(rightMax, height[right]);
        if (height[left] < height[right]) {
            ans += leftMax - height[left];
            ++left;
        } else {
            ans += rightMax - height[right];
            --right;
        }
    }
    return ans;
};
```


## 非力扣题

### 数组扁平化

```
//ES6 flat()
var arr = [1, [2, 3,[4, [5]]]];
arr.flat(Infinity); //[1, 2, 3, 4, 5]

//for 循环递归
var arr = [1, [2, 3,[4, [5]]]];
function flattern(arr) {
    let res = [];
	for (let i=0; i<arr.length; i++){
		if (Array.isArray(arr[i])) {
			res = res.concat(flattern(arr[i])); 
			//或者用扩展运算符 res.push(...flattern(arr[i]))
		} else {
			res.push(arr[i]);
		}
	}
	return res;
}

//reduce() 方法
var arr = [1, [2, 3,[4, [5]]]];
function flattern(arr) {
	return arr.reduce((res, next) => {
		reurn res.concat(Array.isArray(next)? flattern(next) : next);
	}, []);
}
```
