题目描述：  
![image](/basical/IQ/code/countdaystogether.md)  
解决过程：  
ac  
代码：  
```cpp
class Solution {
public:
    unordered_map<int,int> sum;

    int convert(string str) {
        int month = (str[0] - '0') * 10 + str[1] - '0';
        int day = (str[3] - '0') * 10 + str[4] - '0';
        return sum[month] + day;
    }

    int countDaysTogether(string arriveAlice, string leaveAlice, string arriveBob, string leaveBob) {
        vector<int> month = {31,28,31,30,31,30,31,31,30,31,30,31};
        for (int i = 1; i < 12; i++) {
            sum[i+1] = sum[i] + month[i-1];
        }
        int leftAlice = convert(arriveAlice), leftBob = convert(arriveBob);
        int rightAlice = convert(leaveAlice), rightBob = convert(leaveBob);
        if (leftBob > rightAlice || leftAlice > rightBob) return 0;
        return min(rightBob,rightAlice) - max(leftAlice,leftBob) + 1;
    }
};
```