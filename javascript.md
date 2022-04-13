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

## 3  无重复字符的最长子串

给定一个字符串s，请你找出其中不含有重复字符的 最长子串 的长度。

```js
var lengthOfLongestSubstring = function(s) {
    let last_position = new Map()
    let cur_start_pos = 0 
    let max_len = 0

    for(let i in s){
        if( last_position.has( s.charAt(i) ) ){
            //如果前一个重复的字符在当前起点之前，无视他；
            //如果前一个重复的字符在当前起点之后，把起点挪到前一个重复字符后一位
            cur_start_pos = Math.max( cur_start_pos, Number(last_position.get( s.charAt(i) )) + 1 )
        }

        max_len = Math.max(max_len, Number(i) + 1 - cur_start_pos)

        last_position.set(s.charAt(i),i)
    }

    return max_len
};
```
```js
//>90%优化
var lengthOfLongestSubstring = function(s) {
    let last_position = new Map()
    let cur_start_pos = 0 
    let max_len = 0
    for(let i = 0; i<s.length; i++){
        if( last_position.has( s.charAt(i) ) ){
            if(cur_start_pos < last_position.get( s.charAt(i) ) + 1){
                cur_start_pos = last_position.get( s.charAt(i) ) + 1
            }
        }
        max_len = Math.max(max_len, i + 1 - cur_start_pos)
        last_position.set(s.charAt(i),i)
    }
    return max_len
};
```
复杂度：O(N)，其中N是字符串的长度。会遍历整个字符串一次。


## 743. 网络延迟时间
有 n 个网络节点，标记为 1 到 n。

给你一个列表 times，表示信号经过 有向 边的传递时间。 times[i] = (ui, vi, wi)，其中 ui 是源节点，vi 是目标节点， wi 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1 。
示例 1：

![示例1](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

输入：times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
输出：2

解题思路：bell-fordman
```js
/**
 * @param {number[][]} times
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
var networkDelayTime = function(times, n, k) {
    //节点数组，用于存储起点节点到各节点距离
    let vertex = []
    for(let i = 0; i < n; i++){
        //k为起始节点，初始化起始节点到自己距离为0，初始化到其他节点距离为正无穷
        if(i+1 == k){
            vertex[i] = 0
        }else{
            vertex[i] = Number.MAX_SAFE_INTEGER
        }
    }

    //最坏情况每次只更新起点到一个节点的距离，更新全部节点需要n-1次
    for(let i = 1; i < n ; i++){
        for(let v of times){
            //如果现有的从起点到达:"某边终点:v[1]"的距离比从起点到达"该边起点:v[0]"+该边权重远，更新v[1]距离为更短的路径
            if(vertex[v[1]-1] >= vertex[v[0]-1] + v[2]){
                vertex[v[1]-1] = vertex[v[0]-1] +v[2]
            }
        }
    }
    return Math.max(...vertex)==Number.MAX_SAFE_INTEGER?-1:Math.max(...vertex)
};
```