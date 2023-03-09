题目描述：  
![image](/basical/array/image/image15.png)  
解题过程：  
* 借鉴两数之和：先把对前两个数组进行循环，把两个数组两两组合后的相加的值加入哈希表中，然后对后两个数组进行循环之后通过target减去当前的组合的值来查找哈希表中是否存在该值 
代码：  
```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map <int,int> hash;
        for (int a : nums1) {
            for (int b : nums2) {
                ++hash[a + b];
            }
        }
        int result = 0;
        for (int c : nums3) {
            for (int d : nums4) {
                if (hash.count(- c - d))
                    result += hash[- c - d];
            }
        }
        return result;
    }
};
```