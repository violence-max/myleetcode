题目描述：  
![image](/algorithmn/greed/image/image7.png)  
解题过程：  
自己想的时间里面只有暴力解法，然后果断看了答案，让我比较震惊的两个点：取模；对可到达和不可到达的判断。题解真的是太妙了，严格地证明了如果x只能到达y，则x和y之间的点至多也只能到达y。  
代码：  
```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int length = gas.size();
        int i = 0;
        while (i < length) {
            int sumOfGas = 0,sumOfCost = 0;
            int cnt = 0;
            while (cnt < length) {
                int j = (i + cnt) % length;
                sumOfGas += gas[j];
                sumOfCost += cost[j];
                if (sumOfCost > sumOfGas) break;
                cnt++;
            }
            if (cnt == length) {
                return i;
            } else {
                i += (cnt + 1);
            }
        }
        return -1;
    }
};
```