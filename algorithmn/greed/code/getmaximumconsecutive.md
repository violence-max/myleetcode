题目描述：  
![image](/algorithmn/greed/image/image19.png)  
解决过程：  
没有想到解决方案，看了题解。题解很好地考虑了对于一个新的数y，这个y对于已经得到的连续整数区间[0,x]的影响  
代码：    
```cpp
class Solution {
public:
    int getMaximumConsecutive(vector<int>& coins) {
        int res = 1;
        sort(coins.begin(),coins.end());
        for (auto& coin : coins) {
            if (coin > res) {
                break;
            }
            res += coin;
        }
        return res;
    }
};
```