# LeetCode 题解 - 其他

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