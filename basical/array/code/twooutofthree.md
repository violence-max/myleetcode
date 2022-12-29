题目描述：  
![image](/basical/array/image/image30.png)  
解题过程：  
很简单的一道题，我用的集合直接解决的，最后的思路是空间换时间，这道题不用太在意。题解用的位运算和哈希map，第一个数组遍历时与1相或，第二个数组遍历时与2相或，第三个集合遍历的时候与4相或。最终，如果对哈希表中的键值对的值进行与抹去值的末尾的一位后与自身相与仍然存在1则说明至少有两个数组出现过这个值。  
代码：  
```cpp
class Solution {
public:
    void constructSet(unordered_set<int> &set,vector<int> nums) {
        for (int i : nums) {
            set.insert(i);
        }
    }
    vector<int> twoOutOfThree(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3) {
        unordered_set<int> set1,set2,set3;
        constructSet(set1,nums1);
        constructSet(set2,nums2);
        constructSet(set3,nums3);
        vector<int> ans;
        for (int i = 1; i <= 100; i++) {
            if ((set1.find(i) != set1.end() && set2.find(i) != set2.end()) ||
                (set1.find(i) != set1.end() && set3.find(i) != set3.end()) ||
                (set2.find(i) != set2.end() && set3.find(i) != set3.end())
            ) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:
    vector<int> twoOutOfThree(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3) {
        unordered_map<int, int> mp;
        for (auto& i : nums1) {
            mp[i] = 1;
        }
        for (auto& i : nums2) {
            mp[i] |= 2;
        }
        for (auto& i : nums3) {
            mp[i] |= 4;
        }
        vector<int> res;
        for (auto& [k, v] : mp) {
            if (v & (v - 1)) {
                res.push_back(k);
            }
        }
        return res;
    }
};
```