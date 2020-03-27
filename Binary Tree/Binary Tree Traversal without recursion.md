## Description
Traverse Binary Tree with stack.

### Inorder:

    Input：{1,2,3}
    Output：[2,1,3]
    Explanation:
       1
      / \
     2   3
    it will be serialized {1,2,3}
      Inorder Traversal
      
### Preorder:

    Input：{1,2,3}
    Output：[1,2,3]
    Explanation:
       1
      / \
     2   3
    it will be serialized {1,2,3}
    Preorder traversal
    
### Postorder:

    Input：{1,2,3}
    Output：[2,3,1]
    Explanation:  
       1
      / \
     2   3
     it will be serialized {1,2,3}
    Post order traversal
 
 ## Code:
 ### Inorder:
 
     class Solution:
        """
        @param root: A Tree
        @return: Inorder in ArrayList which contains node values.
        """
        def inorderTraversal(self, root):
            # write your code here
            order = []
            if not root:
                return order
            dummy = TreeNode(0)
            dummy.right = root
            stack = [dummy]
            while stack:
                node = stack.pop()
                if node.right:
                    node = node.right
                    while node:
                        stack.append(node)
                        node = node.left
                if stack:
                    order.append(stack[-1].val)

            return order
 
 ### Preorder:
 
     class Solution:
        """
        @param root: A Tree
        @return: Preorder in ArrayList which contains node values.
        """
        def preorderTraversal(self, root):
            # write your code here
            if not root:
                return []
            res, stack = [], [root]
            while stack:
                node = stack.pop()
                res.append(node.val)
                if node.right:
                    stack.append(node.right)
                if node.left:
                    stack.append(node.left)

            return res
 
 ### Postorder:
     class Solution:
        """
        @param root: A Tree
        @return: Postorder in ArrayList which contains node values.
        """
        def postorderTraversal(self, root):
            # write your code here
            if not root:
                return []
            res, stack, node = [], [], root
            while node:
                stack.append(node)
                if node.left:
                    node = node.left
                else:
                    node = node.right
            while stack:
                node = stack.pop()
                res.append(node.val)
                if stack and stack[-1].left == node:
                    node = stack[-1].right
                    while node:
                        stack.append(node)
                        if node.left:
                            node = node.left
                        else:
                            node = node.right

            return res
