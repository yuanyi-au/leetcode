# 套路

## 回溯模板

```
result = [];
function backtrack (path, list) {
    if (满足条件) {
        result.push(path);
        return
    }
    
    for () {
        // 做选择(前序遍历)
        backtrack (path, list)
        // 撤销选择(后续遍历)
    }
}
```

## 单调栈模版

求解下一个最大元素（Next Greater Number）
```
var nextGreaterElement = function (nums) {
    const len = nums.length;
    const result = new Array(len).fill(-1); // 存放答案的数组
    const stack = [];
    for (let i = len - 1; i >= 0; i--) { // 倒着往栈里放
        // 判定高矮
        while (stack.length && stack[stack.length - 1] <= nums[i]) {
            // 矮个起开，反正也被挡着了。。。
            stack.pop();
        }
        // nums[i] 身后的第一个高的
        result[i] = stack.length ? stack[stack.length - 1] : -1;
        // 进队，接受之后的⾝⾼判定吧
        stack.push(nums[i]);
    }
    return result;
}
```

## 递归与迭代的区别

递归是重复调用函数自身实现循环，迭代是函数内部某段代码循环

递归是迭代的一种特殊情况，递归可以用迭代方法写出，反之则不行