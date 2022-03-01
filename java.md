## 1 两数之和

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。


```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        //1.Hashmap的建立复杂度是O(n)也就是线性级别的, 问题规模的增长与输入量成正比
        Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i) {
            //2.哈希表的查找containsKey本质上是使用key去访问哈希表中的value对象，存在则返回true，不存在(null)则返回false，复杂度是常数级别
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            //3.不存在则以此次数值为key，以在数组中序号为value把结果存入哈希表
            hashtable.put(nums[i], i);
        }
        return null;
    }
}
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

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        //1.创建新的头节点和尾节点用于储存结果
        ListNode head = null, tail = null;
        int carry = 0;
        while (l1 != null || l2 != null) {
            //2.判断输入的两个链表对应节点的值是否存在，存在使得n1n2分别等于相应节点的值，不存在则等于0
            int n1 = l1 != null ? l1.val : 0;
            int n2 = l2 != null ? l2.val : 0;
            //3.计算数值之和
            int sum = n1 + n2 + carry;
            if (head == null) {
                //4.head为null时创建第一个节点，并将头尾节点均指向此节点，由于只能保存一位数字，取余以丢弃高位保留最低一位的数值
                head = tail = new ListNode(sum % 10);
            } else {
                //5.创建第二个及以后的节点，首先把当前尾节点的next值指向即将创建的节点
                tail.next = new ListNode(sum % 10);
                //6.更新尾节点的值
                tail = tail.next;
            }
            //7.用carry保存因为进位被丢弃的值，并用于下一次和的计算
            carry = sum / 10;
            //8.如果l1l2链表当前节点不是空节点，将l1l2的指针指向下一个节点用于下一次计算
            if (l1 != null) {
                l1 = l1.next;
            }
            if (l2 != null) {
                l2 = l2.next;
            }
        }
        //9.两个节点都不存在是遍历结束的标志，但是carry大于0需进位，则多创建一个节点
        if (carry > 0) {
            tail.next = new ListNode(carry);
        }
        //10.返回新链表的头节点
        return head;
    }
}
```
复杂度：O(max(m,n))，其中 mm 和 nn 分别为两个链表的长度。我们要遍历两个链表的全部位置，而处理每个位置只需要 O(1)O(1) 的时间。

## 3  无重复字符的最长子串

给定一个字符串s，请你找出其中不含有重复字符的 最长子串 的长度。

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        //1.hashmap用于存储出现某字符的最新位置的值
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                //2.如果j最后出现的位置比i靠后，则更新i的值为此值后面一位，跳过最后出现的位置之前所有的字符
                i = Math.max(map.get(s.charAt(j)) + 1, i);
            }
            ans = Math.max(ans, j - i + 1);
            //3.由于hashmap键不重复的特性，更新后的值是当前字母最后出现的位置
            map.put(s.charAt(j), j);
        }
        return ans;
    }
}
```
复杂度：O(N)，其中N是字符串的长度。左指针和右指针分别会遍历整个字符串一次。

## 4 寻找两个正序数组的中位数

给定两个大小分别为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。请你找出并返回这两个正序数组的 中位数 。

算法的时间复杂度应该为 O(log (m+n)) 。


```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        //1.hashmap用于存储出现某字符的最新位置的值
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                //2.如果j最后出现的位置比i靠后，则更新i的值为此值后面一位，跳过最后出现的位置之前所有的字符
                i = Math.max(map.get(s.charAt(j)) + 1, i);
            }
            ans = Math.max(ans, j - i + 1);
            //3.由于hashmap键不重复的特性，更新后的值是当前字母最后出现的位置
            map.put(s.charAt(j), j);
        }
        return ans;
    }
}
```
复杂度：O(N)，其中N是字符串的长度。左指针和右指针分别会遍历整个字符串一次。