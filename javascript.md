## 1 两数之和
   
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

```javascript
var twoSum = function(nums, target) {
    //利用了js中object天然的哈希特性
    let hashObject = new Object()
    for(i in nums){
        if(hashObject[target - nums[i]] !== undefined){
            return [hashObject[target - nums[i]], i]
        }else{
            hashObject[nums[i]] = i
        }
    }
};
```
复杂度：for循环复杂度是线性级别，由于哈希表的建立复杂度也是线性级别且不存在嵌套，所以此解法时间复杂度是线性级别


## 2 两数相加

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例 1：

输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.

示例 3：

输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
var addTwoNumbers = function(l1, l2) {
    //新建结果链表
    let result = new ListNode()
    //储存头节点
    let result_head = result
    //存储每轮的进位数
    let carry = 0
    while(l1 != undefined || l2 != undefined) {
        let sum = ((l1 != undefined) ? l1.val : 0) + ((l2 != undefined) ? l2.val : 0) + carry
        //每轮除10的余数是该位的结果
        result.val = (sum)%10
        carry = Math.floor(sum/10)
        //如果节点存在就进入下一个节点，不存在就还是不存在（undefined）
        l1 = (l1 != undefined) ? l1.next : undefined
        l2 = (l2 != undefined) ? l2.next : undefined
        //上一步已经进入下一个节点，如果下一个节点存在或者还存在进位数（需要进位）就创建新的结果节点
        if(l1 != undefined|| l2 != undefined){
            result.next = new ListNode()
            result = result.next
        }else if(carry != 0){
            result.next = new ListNode(carry,undefined)
            result = result.next
        }
    }
    //返回头节点
    return result_head
};
```
复杂度：O(max(m,n))，其中 mm 和 nn 分别为两个链表的长度。我们要遍历两个链表的全部位置，而处理每个位置只需要 O(1)O(1) 的时间。

