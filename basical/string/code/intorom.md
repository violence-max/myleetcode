题目描述：  
![image](/basical/string/image/image18.png)  
解题过程： 
这道题是真的不想写，感觉写这种题很没意义，要罗列太多情况了。  
看了题解，感觉又有意义了？一个是根据罗马字符的特点从大到小开始寻找，一个是硬编码表，我觉得把硬编码表写出来还是挺考验人的  
代码： 
```cpp
const pair<int,string> value[] = {
        {1000,"M"},
        {900,"CM"},
        {500,"D"},
        {400,"CD"},
        {100,"C"},
        {90,"XC"},
        {50,"L"},
        {40,"XL"},
        {10,"X"},
        {9,"IX"},
        {5,"V"},
        {4,"IV"},
        {1,"I"},
    };

class Solution {
private:
public:
    string intToRoman(int num) {
        string ans;
        for (int i = 0; i < 13; i++) {
            while (num >= value[i].first) {
                num -= value[i].first;
                ans += value[i].second;
                if (num == 0) return ans;
            }
        }
        return ans;
    }
};
```