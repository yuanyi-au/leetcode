# LeetCode 题解 - 链表

## 简单

### 21. 合并两个有序链表

```
var mergeTwoLists = function(l1, l2) {
    if (l1 == null) {
        return l2;
    } else if (l2 == null) {
        return l1;
    } else if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};
```

### 206. 反转链表

```
var reverseList = function(head) {
    let pre = null;
    let cur = head;
    while (cur) {
        let next = cur.next
        cur.next = pre;
        pre = cur;
        cur = next;
    }
    return pre;
};
```

### 234. 回文链表

```
//用数组辅助
var isPalindrome = function(head) {
    let arr = [];
    while (head) {
        arr.push(head.val);
        head = head.next;
    }
    let i = 0, j = arr.length - 1
    while (i < j) {
        if (arr[i] !== arr[j]) {
            return false;
        }
        i++;
        j--;
    }
    return true;
};
```

### 160. 相交链表

```
//用一个set储存；链表节点
var getIntersectionNode = function(headA, headB) {
    let visited = new Set();
    let temp = headA;
    while (temp) {
        visited.add(temp);
        temp = temp.next;
    }
    temp = headB;
    while (temp) {
        if (visited.has(temp)) {
            return temp;
        }
        temp = temp.next;
    }
    return null;
};
```

### 141. 环形链表

```
//由于map储存的是引用地址，遍历时如果哈希表中存在当前节点则说明有环
var hasCycle = function(head) {
    let visited = new Map();
    while (head) {
        if (visited.has(head)) {
            return true;
        }
        visited.set(head, true);
        head = head.next
    }
    return false;
};

//标准解法是快慢指针，慢指针每次移动一步，快指针每次移动两步，看是否相遇
```