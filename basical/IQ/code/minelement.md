题目描述：  
![image](/basical/IQ/image/image3.png)  
很简单的一道题目，看了题解，这里有一个上整的写法，感觉很秀：(abs(sum - goal) + limit - 1 ) / limit就是答案  
代码:  
```cpp
class Solution {
public:
    int minElements(vector<int>& nums, int limit, int goal) {
        int n = nums.size();
        long long sum = 0;
        for (int n : nums) {
            sum += (long long)n;
        }
        int count = 0;
        count += abs(sum - goal) / limit;
        if (abs(sum - goal) % limit) count++;
        return count;
    }
};
```