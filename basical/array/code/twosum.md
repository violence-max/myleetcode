题目描述：  
![image](/basical/array/image/image14.png)  
解题过程：  
两数之和作为leetcode第一题，自然是比较简单的，很容易想到的就是暴力求解，用一个时间复杂度为O(n^2)的循环法。但是，不应该去使用这种这么简单的方法的，我们应该严格要求自己去使用一些更牛逼的方法。因此，在看了题解之后（原谅我自己没有想出来，最近脑子有点抽），我发现了哈希表这种方法，这个方法是真的无敌，先定义一个哈希表变量，然后从数组第一个数开始进行判断，目标值减去当前值之后的值在不在哈希表中，如果在则直接返回两个值的下标，不在就加入哈希表，你甚至不需要提前把所有值都拿进表里来。  
代码：  
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map <int,int> hash;
        for (int i = 0; i < nums.size(); i++) {
            auto it = hash.find(target - nums[i]);
            if (it != hash.end()) {
                return {it->second,i};
            }
            hash[nums[i]] = i;
        }
        return {};
    }
};
```