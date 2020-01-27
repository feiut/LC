## Description
474. Lowest Common Ancestor II

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

The node has an extra attribute parent which point to the father of itself. The root's parent is null.

Example

Example 1:

    Input：{4,3,7,#,#,5,6},3,5
    Output：4
    Explanation：
         4
         / \
        3   7
           / \
          5   6
    LCA(3, 5) = 4
Example 2:

    Input：{4,3,7,#,#,5,6},5,6
    Output：7
    Explanation：
          4
         / \
        3   7
           / \
          5   6
    LCA(5, 6) = 7
    
## Solution 
### Solution 1: 
见 LeetCode 236. LCA
### Solution 2:
由于已知 parent，所以可以直接从 A node 和 B node 开始往上找 LCA，不需要递归求解。

如果A和B不在同一个高度上，可以把一个点往上沿着parent指针跳到和另一个相同高度的位置。

两个相同高度的点同时沿着各自的parent往上爬，爬到相交的地方就是LCA了。

## Code

    """
    Definition of ParentTreeNode:
    class ParentTreeNode:
        def __init__(self, val):
            self.val = val
            self.parent, self.left, self.right = None, None, None
    """


    class Solution:
        """
        @param: root: The root of the tree
        @param: A: node in the tree
        @param: B: node in the tree
        @return: The lowest common ancestor of A and B
        """
        def lowestCommonAncestorII(self, root, A, B):
            # write your code here

            # Solutin 1: Recursion
            # if not root or root == A or root == B:
            #     return root
            # left = self.lowestCommonAncestorII(root.left, A, B)
            # right = self.lowestCommonAncestorII(root.right, A, B)
            # if not left or not right:
            #     return left if left else right
            # return root


            # Solution 2: Non-recursion
            if root == A or root == B:
                return root

            depthA = self.getDepth(A)
            depthB = self.getDepth(B)
            # make sure A is depper than B
            if depthA < depthB:
                tmp = A
                A = B
                B = tmp
            diff = abs(depthA - depthB)
            # find the lowest common layer of A and B
            while diff > 0:
                A = A.parent
                diff -= 1
            # find the LCA layer by layer
            while A != B:
                A = A.parent
                B = B.parent
            # if A == None and B == None:
            #     return root
            return A

        def getDepth(self, node):
            depth = 0
            while node.parent:
                depth += 1
                node = node.parent
            return depth
