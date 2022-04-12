## Valid Palindrome
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.
```js
var isPalindrome = function(s) {
    proc = s.replace(/[.,\/#!?$%\^&\*;:{}=\-_`~()@'"\[\] ]/g,"").toLowerCase()
    
    if(proc == ''){
        return true
    }
    console.log(proc)
    for(i=0; i<Math.floor(proc.length/2); i++){
        if(proc[i] != proc[proc.length-i-1]){
            return false
        }
    }
    
    return true
};
```
## Reverse String
Write a function that reverses a string. The input string is given as an array of characters s.

You must do this by modifying the input array in-place with O(1) extra memory.
```js
var reverseString = function(s) {
    for(i=0; i<s.length; i++){
        s.splice( i, 0, s.pop() )
    }
    return s
};
```

## 98. Validate Binary Search Tree
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        
        # initial restrictions
        low = -math.inf
        high = math.inf
        
        def validate(node, low, high):
            
            # empty node is valid
            if not node:
                return True
            
            # validate current node 
            if  node.val < high and node.val > low:
                
                # validate both left and right nodes recursively
                return validate(node.left, low, node.val) and validate(node.right, node.val, high)
            
            else:
                return False
        
        
        return validate(root, low, high)
```