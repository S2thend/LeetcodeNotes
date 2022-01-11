## 1 两数之和
   
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        //1.Hashmap的建立复杂度是O(n)也就是线性级别的, 问题规模的增长与输入量成正比
        unordered_map<int, int> hashtable;
        for (int i = 0; i < nums.size(); ++i) {
            //2.哈希表的查找find本质上是使用key去访问哈希表中的value对象，存在则返回一个包含所有结果的迭代器，不存在则返回尾部节点之后的地址，类似null，所以类型比较复杂，使用auto自动类型
            auto it = hashtable.find(target - nums[i]);
            if (it != hashtable.end()) {
                //3.取出并返回迭代器的第一个值和i的值
                return {it->second, i};
            }
            hashtable[nums[i]] = i;
        }
        return {};
    }
};
```
注意：for循环复杂度是线性级别，由于哈希表的建立复杂度也是线性级别且不存在嵌套，所以此解法时间复杂度是线性级别