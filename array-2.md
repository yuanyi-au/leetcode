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

### 46. 全排列

```
//回溯
var permute = function(nums) {
    let ans = [], path = [];
    function backtracking(n, l, used) {
        //回溯终止条件，path长度等于原数组长度
        if (path.length === l) {
            ans.push(path.slice());
            return
        }
        for (let i=0; i<l; i++) {
            if (!used[i]) {
                path.push(n[i]);
                used[i] = true; //标记为用过
                backtracking(n, l, used); //递归
                path.pop(); //状态重置
                used[i] = false; 
            } 
        }
    }
    backtracking(nums, nums.length, []);
    return ans;
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

### 55. 跳跃游戏

```
//贪心，能cover的值如果大于等于数组长度则能跳到终点
var canJump = function(nums) {
    if (nums.length === 1) return true;
    let cover = 0;
    for (let i=0; i<=cover; i++) {
        cover = Math.max(cover, i+nums[i])
        if (cover >= nums.length-1) {
            return true;
        } 
    }
    return false;
};
```

### 45. 跳跃游戏 II

```
//贪心
var jump = function(nums) {
    let curMax = 0; //当前跳跃可达的最远距离
    let nextMax = 0; //下次跳跃可达的最远距离
    let step = 0;
    for (let i=0; i<nums.length-1; i++) {  //注意最后一个位置不需要遍历
        nextMax = Math.max(nextMax, i+nums[i])
        if (i === curMax) {
            curMax = nextMax; 
            step++;
        } 
    }
    return step;
};
```

### 56. 合并区间

```
var merge = function(intervals) {
    intervals.sort((a, b) => a[0] - b[0])
    let pre = intervals[0];
    let ans = [];
    for (let i=0; i<intervals.length; i++) {
        let cur = intervals[i];
        if (cur[0] <= pre[1]) {
            pre[1] = Math.max(cur[1],pre[1]); //合并区间
        } else {
            ans.push(pre);
            pre = cur;
        }
    }
    ans.push(pre);
    return ans;
};
```

### 739. 每日温度

```
//暴力
var dailyTemperatures = function(temperatures) {
    let ans = new Array(temperatures.length).fill(0);
    for (let i=0; i<temperatures.length; i++) {
        for (let j=i+1; j<temperatures.length; j++) {
            if (temperatures[j] > temperatures[i]) {
                ans[i] = j - i;
                break;
            }
        }    
    }
    return ans;
};

//单调栈

```

### 11. 盛最多水的容器

```
var maxArea = function(height) {
    let n = height.length;
    let i = 0, j = n-1;
    let water = 0;
    while (i<j) {
        area = ((j-i) * Math.min(height[i], height[j]));
        water = Math.max(water, area)
        height[i] > height[j] ? j-- : i++;
    }
    return water;
};
```

### 200. 岛屿数量

```
var numIslands = function(grid) {
    let m = grid.length, n = grid[0].length
    function dfs(grid, i, j) {
        //递归终止条件
        if (i<0 || i>=m || j<0 || j>=n || grid[i][j]==='0') {
            return;
        }
        grid[i][j] = '0'; //将走过的标记为0
        dfs(grid, i + 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i - 1, j);
        dfs(grid, i, j - 1);
    }
    let ans = 0;
    for (let i=0; i<m; i++) {
        for (let j=0; j<n; j++) {
            if (grid[i][j] === '1') {
            dfs(grid, i, j);
            ans++;
            }
        }
    }
    return ans;
};
```


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
