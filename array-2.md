# LeetCode 题解 - 数组（2）

## 中等

### 215. 数组中的第K个最大元素

```
//我看人家正常做法是用快排或者堆排序:sweat_smile: 之后再看看
var findKthLargest = function(nums, k) {
    arr = nums.sort((a,b) => b-a)
    return arr[k-1];
};
```

### 15. 三数之和

```
var threeSum = function(nums) {
    const result = [];
    nums.sort((a,b) => a-b);
    for (let i=0; i<nums.length; i++) {
		//去重
        if(i && nums[i] === nums[i - 1]){
            continue;
        }
        let left = i + 1;
        let right = nums.length - 1;
        while (left < right) {
            let sum = nums[i] + nums[left] + nums[right];
            if (sum > 0) {
                right--
            } else if (sum < 0) {
                left++
            } else {
                result.push([nums[i], nums[left++], nums[right--]]);
				//去重
                while(nums[left] === nums[left - 1]){
                    left++;
                }
                while(nums[right] === nums[right + 1]){
                    right--;
                }
            }
        }
    }
    return result;
};
```

### 螺旋矩阵

```
var spiralOrder = function(matrix) {
    const row = matrix.length, col = matrix[0].length;
	const size = row * col;
    const ans = [];
    let t = 0, r = col-1, b = row-1, l = 0; //上右下左
    while (ans.length !== size) {
        //从左到右
        for (let i=l; i<=r; i++) {
            ans.push(matrix[t][i]);    
        } t++;
        //从上到下
        for (let i=t; i<=b; i++) {
            ans.push(matrix[i][r]);           
        } r--;
		//检查越界
		if (ans.length === size) {
		    break;
		}
        //从右到左
        for (let i=r; i>=l; i--) {
            ans.push(matrix[b][i]);
        } b--;
        //从下到上
        for (let i=b; i>=t; i--) {
            ans.push(matrix[i][l]);
        } l++;
    }
    return ans;
};
```

### 跳跃游戏 Ⅱ

```

```


## 困难



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
