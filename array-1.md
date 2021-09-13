# LeetCode 题解 - 数组（1）

## 简单

### 1. 两数之和

```
//暴力
var twoSum = function(nums, target) {
    for(let i=0; i<nums.length; i++){
        let x = nums.indexOf(target - nums[i]);
            if (x != -1 && x != i){ 
                return [i, x]    
            }
    }
    throw Error('No match')
};

//哈希表
var twoSum = function(nums, target) {
    let map = new Map()
    for (let i=0; i<nums.length; i++) {
        let key = target - nums[i];
        if (map.has(key)) {
            return [map.get(key), i]
        }
        map.set(nums[i], i)
    }
    throw Error('No match')
};

```

### 88. 合并两个有序数组

```
//双指针
var merge = function(nums1, m, nums2, n) {
    let i = 0, j = 0;
    nums1.splice(m);
    while (j < n) {
        //如果第一个数组遍历完成，或者对应位置上数组2的值小于数组1的值，执行插入
        if (i >= (m+n) || nums2[j] <= nums1[i]) {
            nums1.splice(i, 0, nums2[j]); //在[i]位置添加nums2[j]
            j++;
        }
        i++;
    }
    return nums1;
};
```

### 121. 买卖股票的最佳时机

```
var maxProfit = function(prices) {
    let result = 0;
    let minPrice = prices[0];

    for (let i=0; i<prices.length; i++){
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        } else if (prices[i] - minPrice > result) {
            result = prices[i] - minPrice
        }
    }
    return result;
};
```

### 53. 最大子序和

```
//动态规划
var maxSubArray = function(nums) {
    let ans = nums[0];
    let sum = 0;
    for (let num of nums) {
        if (sum > 0) {
            sum = num + sum;
        } else {
            sum = num;
        }
        ans = Math.max(ans, sum);
    }
    return ans;
};
```

### 448. 找到所有数组中消失的数字

```
//暴力6780ms :sweat_smile:
var findDisappearedNumbers = function(nums) {
    let res = [];
    for (let i=1; i<=nums.length; i++) {
        if (nums.indexOf(i) === -1) {
            res.push(i);
        }
    }
    return res
};

//Set 去重
var findDisappearedNumbers = function(nums) {
    let s = new Set();
    for (let i=0; i<nums.length; i++){
        s.add(nums[i])
    }
    for (let j=1; j<=nums.length; j++){
        if (s.has(j)) {
            s.delete(j);
        } else {
            s.add(j);
        }
    }
    return Array.from(s);
};
```

### 704. 二分查找

```
var search = function(nums, target) {
    let left = 0, right = nums.length - 1;
    while (left <= right) {
        let mid = ((right + left) >> 1);
        if (target == nums[mid]) {
            return mid;
        } else if (target < nums[mid]){
            right = mid - 1;
        } else {
            left = mid +1;
        } 
    }
    return -1;
};
```

### 35. 搜索插入位置

```
//暴力
var searchInsert = function(nums, target) {
    for (let i=0; i<nums.length; i++) {
        if ( nums[i] >= target) {
            return i
        }
    } 
    return nums.length
};

//二分查找
var searchInsert = function(nums, target) {
    const n = nums.length;
    let left = 0, right = n - 1, ans = n;
    while (left <= right) {
        let mid = ((right - left) >> 1) + left; 
        //用 ((right - left) / 2) 没有办法通过，搞不懂为什么
        if (target <= nums[mid]) {
            ans = mid;
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return ans;
};
```

### 27. 移除元素

```
//双指针
var removeElement = function(nums, val) {
    let left = 0;
    for (let right=0; right<nums.length; right++) {
        if (nums[right] !== val) {
            nums[left] = nums[right];
            left++;
        }
    }
    return left;
};
```

### 26. 删除有序数组中的重复项

```
//双指针
var removeDuplicates = function(nums) {
    let left = 0;
    for (right=1; right<nums.length; right++) {
        if (nums[right] !== nums[left]){
            left++
            nums[left] = nums[right];
        }
    }
    return left + 1;
};
```

### 283. 移动零

```
//双指针
var moveZeroes = function(nums) {
    let left = 0, right = 0
    while (right < nums.length) {
        if (nums[right] !== 0) {
            let temp = nums[right];
            nums[right] = nums[left];
            nums[left] = temp;
            left++;
        }
        right++;
    }
    return nums;
};
```

### 169. 多数元素

```
//排序后数组最中间的数必然是多数元素
var majorityElement = function(nums) {
    let arr = nums.sort();
    return arr[Math.floor(arr.length / 2)]
};
```

### 136. 只出现一次的数字

```
//按位异或
var singleNumber = function(nums) {
    let ans = 0;
    for (let num of nums) {
        ans ^= num;
    }
    return ans;
};
```

