题目描述：  
![image](/basicaldatastructure/stackandquere/image/image12.png)  
解决过程：  
想得好复杂啊，兜兜转转回到了暴力，嘻嘻  
看了题解，原来就是先算出nums2的单调栈然后用哈希表存储结果，最后遍历一次nums1生成结果  
代码：  
```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(),n = nums2.size();
        unordered_map<int,int> map;
        int index = 0;
        for (int n : nums2) {
            map[n] = index++;
        }
        vector<int> ans(m,-1);
        for (int j = 0; j < m; j++) {
            for (int i = map[nums1[j]] + 1; i < n; i++) {
                if (nums2[i] > nums1[j]) {
                    ans[j] = nums2[i];
                    break;
                }
            }
        }
        return ans;
    }
};
```