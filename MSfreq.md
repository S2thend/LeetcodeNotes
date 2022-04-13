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


## 743. 网络延迟时间
有 n 个网络节点，标记为 1 到 n。

给你一个列表 times，表示信号经过 有向 边的传递时间。 times[i] = (ui, vi, wi)，其中 ui 是源节点，vi 是目标节点， wi 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1 。
示例 1：
![示例 1](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)
输入：times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
输出：2
```py
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        # initialize vertexes list for storing distance from begining vertex 
        vertexes = [ 0 if i+1 == k else math.inf for i in range(n)]

        # SEE BELOW for explaination of ABOVE init process 
        # # n is the number of vertexes
        # for i in range(0,n):
        #     # k is begining vertex's place of order
        #     if i+1 == k:
        #         # distance is 0 from begining to begining
        #         vertexes[i] = 0 
        #     else:
        #         # all other distance begins with infinite
        #         vertexes[i] = math.inf

        # RELAXATION OF EDGES
        # worst case is we can find distances one vertex at a time, 
        # so we need go n-1 loops in worst case
        # this is also important for update because we check weight from every begining point of a edge 
        for i in range(1,n):
            # "times" equals to edges, stores begin[0],end[1],weights[2] 
            for edge in times:
                # if going through this current edge shorter the path then update
                if( vertexes[ edge[1] - 1 ] > vertexes[ edge[0] - 1 ] + edge[2] ):
                    vertexes[ edge[1] - 1 ] = vertexes[ edge[0] -1 ] + edge[2]

        return -1 if max(vertexes) == math.inf else max(vertexes)
```