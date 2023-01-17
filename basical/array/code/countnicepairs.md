题目描述：  
![image](/basical/array/image/image38.png)  
解题过程：  
靠，这道题一开始模拟，直接超时。做的时候犯了两次病，一次手滑什么都没写按了提交，一次刚改了代码没测试也提交了。本来直接模拟的时间复杂度是O(n^2)，在第一次超时之后我想着要降低时间复杂度嘛，就用一个数组把反转的结果记录起来，结果还是超时。我就觉得应该有方法可以降到O(nlog(n))，但是二分查找又不符合计数的题目，先排序过后也没什么直观的发现。果断看题解，发现可以把等式变化，变成nums[i]-rev(nums[i])，即抽象出一个函数，一次遍历，遍历到每个数的时候统计这个数对应的结果已经出现过几次了，这个实现用一个哈希表就可以了，然后把值加到答案里面去。真牛啊这个方法，就是有点费空间。  
代码：  
```cpp
class Solution {
public:
    int countNicePairs(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int,int> map;
        int ans = 0,mod = 1e9 + 7;
        for (int i : nums) {
            int temp = i, j = 0;
            while (temp) {
                j = j * 10 + temp % 10;
                temp /= 10;
            }
            ans = (ans + map[i-j]) % mod;
            map[i-j]++;
        }
        return ans;
    }
};
```