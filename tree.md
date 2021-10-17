# LeetCode 题解 - 树
- [LeetCode 题解 - 树](#leetcode-题解---树)
  - [简单](#简单)
    - [94. 二叉树的中序遍历](#94-二叉树的中序遍历)
    - [104. 二叉树的最大深度](#104-二叉树的最大深度)
    - [543. 二叉树的直径](#543-二叉树的直径)
    - [101. 对称二叉树](#101-对称二叉树)
    - [617. 合并二叉树](#617-合并二叉树)
    - [226. 翻转二叉树](#226-翻转二叉树)
  - [中等](#中等)
    - [102. 二叉树的层序遍历](#102-二叉树的层序遍历)
    - [98. 验证二叉搜索树](#98-验证二叉搜索树)
    - [96. 不同的二叉搜索树](#96-不同的二叉搜索树)
    - [105. 从前序与中序遍历序列构造二叉树](#105-从前序与中序遍历序列构造二叉树)

## 简单

### 94. 二叉树的中序遍历

```
var inorderTraversal = function(root) {
    let ans = [];
    function inOrder(root) {
        if (root == null) {
            return;
        }
        inOrder(root.left);
        ans.push(root.val);
        inOrder(root.right);
    }
    inOrder(root);
    return ans;
};
```

### 104. 二叉树的最大深度

```
//DFS
var maxDepth = function(root) {
    if (root == null) {
        return 0;
    }
    let left = maxDepth(root.left);
    let right = maxDepth(root.right);
    return Math.max(left, right) + 1
};
```

### 543. 二叉树的直径

```
var diameterOfBinaryTree = function(root) {
    let ans = 1;
    function depth(root) {
        if (root == null) {
            return 0;
        }
        let left = depth(root.left);
        let right = depth(root.right);
        //获取最长路径
        ans = Math.max(ans, left + right + 1);
        return Math.max(left, right) + 1;
    }
    depth(root);
    return ans - 1;
};
```

### 101. 对称二叉树

```
var isSymmetric = function(root) {
    if (root == null) {
        return true;
    }
    function compare(left, right) {
        if (left == null && right !== null || left !== null && right == null) {
            return false;
        } else if (left == null && right == null){
            return true;
        } else if (left.val !== right.val) {
            return false;
        }
        //单层递归，比较外侧和内侧是否对称
        let outside = compare(left.left, right.right);
        let inside = compare(left.right, right.left);
        return outside && inside;
    }
    return compare(root.left, root.right);
};
```

### 617. 合并二叉树

```
//前序遍历
var mergeTrees = function(root1, root2) {
    function preOrder(root1, root2) {
        if (root1 == null) {
            return root2;
        }
        if (root2 == null) {
            return root1;
        }
        root1.val = root1.val + root2.val;
        root1.left = preOrder(root1.left, root2.left);
        root1.right = preOrder(root1.right, root2.right);
        return root1;
    }
    return preOrder(root1, root2);
};
```

### 226. 翻转二叉树

```
var invertTree = function(root) {
    if (root == null) {
        return null;
    }
    let temp = invertTree(root.left);
    root.left = invertTree(root.right);
    root.right = temp;
    return root;
};
```

## 中等

### 102. 二叉树的层序遍历

```
var levelOrder = function(root) {
    if (root == null) return [];
    const ans = [];
    const queue = [root];
    while (queue.length > 0) {
        let arr = [];
        let l = queue.length;
        while (l--) {
            let node = queue.shift();
            arr.push(node.val);
            if (node.left !== null) queue.push(node.left);
            if (node.right !== null) queue.push(node.right);
        }
        ans.push(arr)
    }
    return ans;
};
```

### 98. 验证二叉搜索树

```
//二叉搜索树中序遍历得到的数组必然是增序的
var isValidBST = function(root) {
    let arr = [];
    function inOrder(root) {
        if (root !== null) {
            inOrder(root.left);
            arr.push(root.val);
            inOrder(root.right);
        }
    }
    inOrder(root);
    for (let i=1; i<arr.length; i++) {
        if (arr[i] <= arr[i-1]) {
            return false;
        }
    }
    return true;
};
```

### 96. 不同的二叉搜索树

```
//卡特兰数公式
var numTrees = function(n) {
    let dp = new Array(n+1).fill(0);
    dp[0] = 1; dp[1] = 1;
    for (let i=2; i<=n; i++) {
        for (let j=1; j<=i; j++) {
            dp[i] += dp[j-1] * dp[i-j];
        }
    }
    return dp[n];
};
```

### 105. 从前序与中序遍历序列构造二叉树

```
var buildTree = function(preorder, inorder) {
    if (!inorder.length) return null;
    let node = new TreeNode(preorder[0]); //创建根节点
    let mid = inorder.indexOf(preorder.shift()) //preoder[0]对应inorder中的位置
    //左右子树递归
    node.left = buildTree(preorder, inorder.slice(0, mid));
    node.right = buildTree(preorder, inorder.slice(mid + 1));
    return node;
};
```