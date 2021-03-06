# 105. Construct Binary Tree from Preorder and Inorder Traversal - Medium

**从前序与中序遍历序列构造二叉树**

根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

**所有可能解法**

**CODINNG**

*递归解法*
```
class Solution {
    //分别构建每个子树的遍历，选好头结点和尾节点。
    func helper(_ preorder: [Int], _ inorder: [Int], preStart: Int, preEnd: Int, inStart: Int, inEnd: Int) -> TreeNode? {

        if inStart > inEnd {
            return nil
        }

        if preStart > preEnd {
            return nil
        }
        var inIndex = 0
        //先根遍历的第一个是root，找到其在中根中的位置，将树分为左和右子树
        let cur = preorder[preStart]
        for i in inStart...inEnd {
            if cur == inorder[i] {
                inIndex = i
                break
            }
        }

        let root = TreeNode.init(cur)
        let leftLength = inIndex - inStart //左子树元素个数
        let rightLength = inEnd - inIndex //右子树元素个数
        //分别对左子树递归操作，同理，此时并无法确切知道左子树在先根的范围，要算,preStart + leftLength
        let left = helper(preorder, inorder, preStart: preStart + 1, preEnd: preStart + leftLength, inStart: inStart, inEnd: inIndex - 1)
        //此时并不知道右子树在先根的范围，要算：preEnd - rightLength + 1
        let right = helper(preorder, inorder, preStart: preEnd - rightLength + 1, preEnd: preEnd, inStart: inIndex + 1, inEnd: inEnd)
        //重建二叉树
        root.left = left
        root.right = right
        return root
    }

    func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        if 0 == preorder.count || 0 == inorder.count {
            return nil
        }

        return helper(preorder, inorder, preStart: 0, preEnd: preorder.count - 1, inStart: 0, inEnd: inorder.count - 1)
    }
}
```
