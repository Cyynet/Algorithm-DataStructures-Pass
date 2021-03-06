# 98. Validate Binary Search Tree - Medium
**验证二叉搜索树**

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：
- 节点的左子树只包含小于当前节点的数。
- 节点的右子树只包含大于当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

```
示例 1:

输入:
    2
   / \
  1   3
输出: true

示例 2:

输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。
```

**可能解法**

- DFS。
- 中序遍历，查看是否是升序排列。

**CODINNG**

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     public var val: Int
 *     public var left: TreeNode?
 *     public var right: TreeNode?
 *     public init(_ val: Int) {
 *         self.val = val
 *         self.left = nil
 *         self.right = nil
 *     }
 *

class Solution {
    func isValidBST(_ root: TreeNode?, min: Int, max: Int) -> Bool {
        guard let safeRoot = root else { return true }

        if safeRoot.val <= min {
            return false
        }

        if safeRoot.val >= max {
            return false
        }

        return isValidBST(safeRoot.left, min: min,max: safeRoot.val) && isValidBST(safeRoot.right, min: safeRoot.val, max: max)
    }
    func isValidBST(_ root: TreeNode?) -> Bool {
        guard let safeRoot = root else { return true }

        return isValidBST(safeRoot, min: Int.min, max: Int.max)
    }
    
    //非递归
    func isValidBST(_ root: TreeNode?) -> Bool {
        guard root != nil else {
            return true
        }
        var stack = [(root: TreeNode?, lower: Int, upper: Int)]()
        stack.append((root: root, lower: Int.min, upper: Int.max))
        while !stack.isEmpty {
            let element = stack.removeLast()
            if element.root == nil {
                continue
            }
            if element.lower >= element.root!.val || element.upper <= element.root!.val  {
                return false
            }
            stack.append((root: element.root!.left, lower: element.lower, upper: element.root!.val))
            stack.append((root: element.root!.right, lower: element.root!.val, upper: element.upper))
        }
        return true
    }
}

```
