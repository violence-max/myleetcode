题目描述：  
![image](/basical/string/image/image21.png)  
解题过程：  
我操了，这种弱智题目花了二十分钟，看来还是对字符串不熟悉啊，主要卡在字母字符串转换成对应的数字字符串上了，对于to_string()函数的使用不是很了解，一开始写的to_string(ch-’a’+’1’)，然而ch-’a’就已经是一个整型数据了，加上一个’1’我也不知道是什么东西，后面就把’1’改成1了  
代码：  
```cpp
class Solution {
public:
    int luckyString(string s,int time) {
        int sum = 0;
        for (char c : s) {
            sum += c - '0';
        }
        if (time == 1) {
            return sum;
        } else {
            return luckyString(to_string(sum),time-1);
        }
    }
    int getLucky(string s, int k) {
        string str;
        for (char c : s) {
            str += to_string(c - 'a' + 1);
        }
        return luckyString(str,k);
    }
};
```