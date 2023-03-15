## 翻转二叉树

[leetcode题目传送门](https://leetcode.cn/problems/invert-binary-tree/)

1. 题目描述

    ![翻转二叉树](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

2. 思路

    按顺序依次交换二叉树的左右节点


3. 实现

    交换左右节点，递归遍历

```c#

public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;
    public TreeNode(int val=0, TreeNode left=null, TreeNode right=null) {
       this.val = val;
         this.left = left;
         this.right = right;
     }
 }

public class Solution {
    public TreeNode InvertTree(TreeNode root) {
        if(root==null){
            return null;
        }

        // 暂存，左右交换，通过递归实现子节点的自己交换
        TreeNode temp = root.left;

        root.left=InvertTree(root.right);

        root.right=InvertTree(temp);

        return root;

    }

}



```

