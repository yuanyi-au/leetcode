# LeetCode 题解 - 字符串
- [LeetCode 题解 - 字符串](#leetcode-题解---字符串)
  - [简单](#简单)
    - [415. 字符串相加](#415-字符串相加)
    - [9. 回文数](#9-回文数)
    - [14. 最长公共前缀](#14-最长公共前缀)
    - [13. 罗马数字转整数](#13-罗马数字转整数)
    - [20. 有效的括号](#20-有效的括号)
    - [58. 最后一个单词的长度](#58-最后一个单词的长度)
  - [中等](#中等)
    - [139. 单词拆分](#139-单词拆分)
    - [3. 无重复字符的最长子串](#3-无重复字符的最长子串)
    - [5. 最长回文子串](#5-最长回文子串)
    - [93. 复原 IP 地址](#93-复原-ip-地址)
    - [22. 括号生成](#22-括号生成)
    - [1190. 反转每对括号间的子串](#1190-反转每对括号间的子串)
    - [394. 字符串解码](#394-字符串解码)
    - [43. 字符串相乘](#43-字符串相乘)

## 简单

### 415. 字符串相加

```
// 双指针
var addStrings = function(num1, num2) {
    var ans = [];
    let ten = 0;
    let i = num1.length - 1; j = num2.length - 1;
    while (i >= 0 || j >= 0 || ten !== 0) {
        var a = i >= 0 ? num1[i] - '' : 0;
        var b = j >= 0 ? num2[j] - '' : 0;
        var sum = a + b + ten;
        ans.unshift(sum % 10);
        ten = (sum / 10) | 0;
        i--; j--;
    }
    return ans.join('');
};
```

### 9. 回文数

```
//双指针
var isPalindrome = function(x) {
    if (x<0) return false;
    let num = x.toString();
    let i=0; 
    let j=num.length-1;
    while (i<j) {
        if (num[i] !== num[j]) return false;
        i++; j--;
    } 
    return true;
};

//reverse
var isPalindrome = function(x) {
    let rev = x.toString().split("").reverse().join("");
    if (rev == x) return true;
    else return false;
};
```

### 14. 最长公共前缀

```
var longestCommonPrefix = function(strs) {
    let row = strs.length;
    let col = strs[0].length;
    for (let i=0; i<col; i++) {
        let head = strs[0][i];
        //纵向扫描
        for (let j=1; j<row; j++) {
            if (strs[j].length == i || strs[j][i] !== head) {
                return strs[0].substr(0, i);
            }
        }
    }
    return strs[0];
};
```

### 13. 罗马数字转整数

```
// 哈希表
var romanToInt = function(s) {
    const map = {
        I : 1,
        IV: 4,
        V: 5,
        IX: 9,
        X: 10,
        XL: 40,
        L: 50,
        XC: 90,
        C: 100,
        CD: 400,
        D: 500,
        CM: 900,
        M: 1000
    }
    let sum = 0;
    for (let i=0; i<s.length; i++) {
        var n = map[s[i]];
        var next = map[s[i+1]];
        if (n < next) {
            sum += next - n;
            i = i +1;
        } else {
            sum += n
        }
    }
    return sum;
}
```

### 20. 有效的括号

```
//栈 + 哈希表
var isValid = function(s) {
    const stack = [];
    const map = {
        '(' : ')',
        '{' : '}',
        '[' : ']'
    }
    for ( n of s) {
        if (n in map) {
            stack.push(n);
            continue;
        }
        if (map[stack.pop()] !== n) {
            return false;
        }
    }
    return !stack.length
};
```

### 58. 最后一个单词的长度

```
var lengthOfLastWord = function(s) {
    let str = s.split("").reverse().join("");
    let count = 0;
    let i=0;
    while (i<str.length && str[i] == " ") i++;
    while (i<str.length && str[i] !== " ") {
        count++;
        i++;
    }
    return count;
};
```

## 中等

### 139. 单词拆分

```
//动态规划
var wordBreak = function(s, wordDict) {
    let dp = new Array(s.length).fill(false);
    let set = new Set(wordDict);
    //判断0-i之间的字符是否存在于 wordDict中
    for (let i=0; i<s.length; i++) {
        if (set.has(s.substring(0, i+1))) {
            dp[i] = true;
            continue;
        }
        //判断判断j-i之间的字符串是否存在于wordDict中
        for (let j=0; j<i; j++) {
            if (dp[j] && set.has(s.substring(j+1, i+1))) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[s.length-1];
};
```

### 3. 无重复字符的最长子串

```
//暴力
var lengthOfLongestSubstring = function(s) {
    let result = 0;
    for (let i=0; i<s.length; i++) {
        let set = new Set();
        let m = 0;
        let j = i;
        while (j<s.length && !set.has(s[j])) {
            set.add(s[j]);
            m++; j++;
        }
        result = Math.max(result, m);
    }
    return result;
};

//滑动窗口
var lengthOfLongestSubstring = function(s) {
    let result = 0;
    let set = new Set();
    let left = 0, right = 0;

    while (left < s.length) {
        //如果字符不重复就扩大窗口
        while (right < s.length && !set.has(s[right])) {
            set.add(s[right]);
            right++;
        }
        result = Math.max(result, right - left);
        //收缩窗口
        set.delete(s[left]);
        left++;
    }
    return result;
};
```

### 5. 最长回文子串

```
//中心扩散法
var longestPalindrome = function(s) {
    if (s.length < 2) {
        return s;
    }
    let ans = '';
    for (let i=0; i<s.length; i++) {
        center(i, i); //回文串长度为奇数
        center(i, i+1); //回文串长度为偶数
    }

    function center(m, n) {
        while (m>=0 && n<s.length && s[m] == s[n]) {
            m--; n++;
        }
        //取[m+1,n-1]的区间，长度为(n-m-1)
        if (n - m - 1 > ans.length) {
            ans = s.slice(m +1, n)
        }
    }
    return ans;
};
```

### 93. 复原 IP 地址

```
//回溯
var restoreIpAddresses = function(s) {
    let ans = [], path = [];
    function backtracking(i) {
        let len = path.length;
        if(len>4) return;
        //划分为4段
        if(len === 4 && i === s.length) {
            ans.push(path.join("."));
            return;
        }
        for(let j=i; j<s.length; j++) {
            let str = s.substr(i, j-i+1);
            if(str.length > 3 || str > 255) break;
            if(str.length > 1 && str[0] === "0") break;
            path.push(str);
            backtracking(j+1);
            path.pop();
        }
    }
    backtracking(0, 0);
    return ans;
};
```

### 22. 括号生成

```
//回溯算法 DFS
//当剩下的 ) 比 ( 多时，才可以选 )

var generateParenthesis = function(n) {
    let ans = [];
    function dfs(current, left, right) {
        //递归出口
        if (current.length == 2*n) {
            return ans.push(current);
        }
        if (left < n) {
            dfs (current + "(", left + 1, right);
        }
        if (right < left) {
            dfs (current + ")", left, right + 1);
        }
    }
    //递归入口
    dfs("", 0, 0);
    return ans;
};
```

### 1190. 反转每对括号间的子串

```
// 遇到左括号就入栈，遇到右括号就出栈
var reverseParentheses = function(s) {
    const stack = [];
    let reverseStr = '';
    for (let str of s) {
        if (str == '(') {
            stack.push(reverseStr);
            reverseStr = '';
        } else if (str == ')') {
            reverseStr = reverseStr.split("").reverse().join("");
            reverseStr = stack[stack.length - 1] + reverseStr;
            stack.pop();
        } else {
            reverseStr += str;
        }
    }
    return reverseStr;
};
```

### 394. 字符串解码

```
var decodeString = function(s) {
    let numStack = [];
    let strStack = [];
    let num = 0;
    let result = '';
    for (const str of s) {
        if (!isNaN(str)) {
            num = num * 10 + Number(str); // 算出倍数
        } else if (str == '[') {
            strStack.push(result); //遇到左括号字符入栈
            result = ''; // 入栈后清零
            numStack.push(num); //倍数入栈
            num = 0; // 入栈后清零
        } else if (str == ']') {  // 遇到右括号两个栈顶出栈
            let repeatTimes = numStack.pop(); // 获取拷贝次数
            result = strStack.pop() + result.repeat(repeatTimes); //构建子串
        } else {                   
            result += str; //如果后面的还是字母，追加给result串
        }
    }
    return result;
};
```

### 43. 字符串相乘

```
var multiply = function(num1, num2) {
    if (num1 === '0' || num2 === '0') return '0'
    let l1 = num1.length, l2 = num2.length, pos = new Array(l1 + l2).fill(0)
    for (let i = l1; i--;) {
        for (let j = l2; j--;) {
            let tmp = num1[i] * num2[j] + pos[i + j + 1]
            pos[i + j + 1] = tmp % 10   //个位取模
            pos[i + j] += 0 | tmp / 10  //十位取整
        } 
    }
    //删除多余的0
    while(pos[0] === 0) {
        pos.shift()
    }
    return pos.join('')
};
```

