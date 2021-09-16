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

## 中等

### 2. 两数相加

```
var addTwoNumbers = function(l1, l2) {
    let addOne = 0; //表示进位
    let sum = new ListNode('0');
    let head = sum;
    while (addOne || l1 || l2) {
        let val1 = l1 !== null ? l1.val : 0;
        let val2 = l2 !== null ? l2.val : 0;
        let r1 = val1 + val2 + addOne; //求和
        addOne = r1 >= 10 ? 1 : 0; //进位
        sum.next = new ListNode(r1 % 10);
        sum = sum.next;
        if (l1) l1 = l1.next;
        if (l2) l2 = l2.next;
    }
    return head.next; //head中保存的第一个节点是刚开始定义的“0”
};
```

### 143. 重排链表

```
var reorderList = function(head) {
    let node = head;
    //用数组把节点储存下来
    let list = [];
    while (node) {
        list.push(node);
        node = node.next;
    }
    //双指针遍历
    let i = 1, j = list.length - 1;
    head.next = null;
    let current = head;
    while (i<j) {
        list[j].next = list[i];
        list[i].next = null;
        current.next = list[j];
        current = list[i];
        i++; j--;
    }
    if (i===j) {
        list[i].next = null;
        current.next = list[i];
    }
    return list;
}; 
```

### 92. 反转链表 II

```

```