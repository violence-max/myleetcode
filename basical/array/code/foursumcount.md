题目描述：  
![image](/basical/array/code/foursumcount.md)  
解题过程：  
有一说一，一看到这种题就有点头疼，不知道从哪里下手。一开始想了一个暴力解法，但是时间复杂度是O(n^4)，就没有继续弄下去了。然后想了一个分组的方法，想想能不能把时间复杂度降到O(n^2)，就是先把前面两个数组的值先存进哈希表里，然后再对后两个数组的值的和做循环，但是我上一个题做的是两数之和，就有种固有思想，想着如果不用提前存数而是临时加入哈希表的值应该更加好，但是我想着想着就头疼欲裂就看了题解，看了题解发现，确实牛逼，就是一个O(n^2)的解法而已。我实现的时候还在那犯糊涂，哎，找到了具体对应的值后结果居然只加一，应该加的是对应值在哈希表里的次数啊。  
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