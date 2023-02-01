题目描述：  
![image](/basical/IQ/image/image22.png)  
解决过程：  
一开始想的是从9到0，再从个位数到10^9位数，用优先队列把大的值给存储起来，然后用大的值来拼接字符串。但是对于3和34这种数字，这个方法会得到334而不是343，因此不太行。看了题解，**自定义排序**可以很好的解决这个问题。题解里面有具体的证明，复习的时候如果时间充足可以看一下，代码写得很清楚，主要看的还是代码  
代码：  
```cpp
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end(), [](const int& x, const int& y) {
            long sx = 10, sy = 10;
            while (sx <= x) sx *= 10;
            while (sy <= y) sy *= 10;
            return x * sy + y > y * sx + x;
        });
        if (nums[0] == 0) {
            return "0";
        }
        string ans;
        for (int& num : nums) {
            ans += to_string(num);
        }
        return ans;
    }
};
```