题目描述：  
![image](/basical/array/image/image11.png)
解题过程：  
这种题目刚看第一眼真的会以为只有遍历一种解法，完全想不出来另外一种解法，甚至我连暴力求解都想不出来具体的细节和方法，有可能是因为之前做过三数之和那道题目留给我的后遗症吧，总之就是感觉遍历有点tricky，但是实际上只是一个O(n^2)的遍历问题而已，双循环，看了题解以后发现很简单，那个暴力求法。  
题解给出了两种优化解，一种是前缀和+二分法，使用O(n)遍历数组中每一个数，使用O(log(n))来找出最小下标，这种方法有点抽象，但是还行；另一种方法是滑动窗口，这是最tricky的方法！！！很强，很简洁，一看代码就会，一滑就过去了。  
直接看代码吧：  
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int length = nums.size();
        if (length == 0) return 0;
        int ans = INT_MAX;
        int start = 0,end = 0;
        int sum = 0;
        while (end < length) {
            sum += nums[end];
            while (sum >= target) {
                ans = min(ans,(end - start + 1));
                sum -= nums[start++];
            }
            end++;
        }
        return (ans == INT_MAX) ? 0 : ans;
    }
};
```