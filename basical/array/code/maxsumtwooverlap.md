题目描述：  
![image](/basical/array/image/image71.png)  
解决过程：  
暴力超时了  
- 滑动窗口、动态规划：
1. 从下标0和firstlen开始，分别选取长度为firstlen和secondlen的窗口，并且计算总和作为初始值
2. 分别使用一个变量去滑动这两个窗口，对于左边的窗口设置一个变量，总是维护其最大值，两个窗口之和的最大值即为答案
3. 关键思路：不妨考虑长度为firstlen的窗口在长度为secondlen的窗口的左侧，在保持两个窗口不重叠的基础上总是选取左侧窗口的最大值，然后遍历右侧的窗口（相较于暴力那种On^2的时间复杂度的坏方法的优劣性自己考量）  
代码：  
```cpp
class Solution {
public:
    int help(vector<int>& nums, int firstLen, int secondLen) {
        int suml = accumulate(nums.begin(), nums.begin() + firstLen, 0);
        int maxL = suml;
        int sumr = accumulate(nums.begin() + firstLen, nums.begin() + firstLen + secondLen, 0);
        int sum = maxL + sumr;
        for (int i = firstLen + secondLen, j = firstLen; i < nums.size(); i++, j++) {
            suml += nums[j] - nums[j - firstLen];
            maxL = max(suml, maxL);
            sumr += nums[i] - nums[i - secondLen];
            sum = max(sum, maxL + sumr);
        }
        return sum;
    }
    int maxSumTwoNoOverlap(vector<int>& nums, int firstLen, int secondLen) {
        return max(help(nums,firstLen,secondLen), help(nums,secondLen,firstLen));
    }
};
```