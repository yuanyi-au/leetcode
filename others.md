# LeetCode 题解 - 其他

## 70. 爬楼梯

```
var climbStairs = function(n) {
    let dp = [];
    //dp[0]没有意义，直接初始化dp[1]和dp[2]
    dp[1] = 1;
    dp[2] = 2;
    for (let i=3; i<=n; i++) {
        ////爬 i 层楼梯的方式数 = 爬 i-2 层楼梯的方式数 + 爬 i-1 层楼梯的方式数
        dp[i] = dp[i-1] + dp[i-2]; 
    }
    return dp[n];
};
```

##  Excel表列名称

```
var convertToTitle = function(n) {
    let res = '';
    while( n ) {
        const num = n % 26 ? n % 26 : 26; // 取模； 0则取26
        res = String.fromCharCode( num + 65 - 1 ) + res; // 获取A-Z字符串
        n = parseInt( (n-num) / 26 ); // 取余
    }
    return res;
};
```