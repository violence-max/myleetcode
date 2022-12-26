题目描述：  
![image](/basical/string/image/image31.png)  
解题过程：  
这个题需要讨论的情况太多了，由于我不会状态机，所以我只能ifelse语句来写了。  
从i=0开始讨论：  
1. 如果第一位是正负号，则i前进一位，如果此时到达数字串末尾，由于不允许只具有正负号，返回false
2. 用start记录i，为之后的整数部分做铺垫
3. 开始第一次大范围地移动i：在遇到小数点或者字符串末尾之前一直进行i位置上的字符是否为整数的判断（现在知道可以用api函数**isdigit(char c)**），如果是就继续移动数字，不是就直接返回false
4. 第一次大范围地移动i退出while循环地结果有三个：
    1. 到达了字符串的末尾，说明字符串是一个整数，返回true
    2. 遇到了小数点
    3. 遇到了指数符号
5. 若遇到了指数符号，如果指数符号之前没有数字，即i与start相等，则返回false；否则对指数符号之后的数字进行判断是否合法：先将i移动到下一位，如果移动到了字符串的末尾，则说明指数符号后面没有数字，返回false；否则判断i位是否为正负号，如果是则再移动一位否则不做改变；如果存在正负号并且i移动到了字符串的最后一位仍然说明指数后面不存在数字，返回false；否则对i之后的字符进行是否为整数的判断，若在到达字符串末尾之前遇到了非整数则返回false，否则顺利到达字符串尾部并且返回true
6. 用digit记录小数点的位置（因为题目中对小数的描述里面有一条：一点小数点后面跟着至少一位数字也是小数，所以在进入指数部分的讨论之前需要对小数点的位置进行记录，防止在出现指数符号之前并没有一个小数或者整数的出现）
7. i移动到下一位，如果此时移动到了字符串的末尾并且i-1位大于start，说明字符串是一个小数，但若此时移动到了字符串的末尾并且i-1位与start相等，说明小数点前后并无数字，返回false
8. 用start2记录i，为之后的指数部分做铺垫
9. 开始第二次大范围地移动i：在遇到指数符号或者移动到字符串末尾之前一直进行i位置上的字符是否为整数的判断
10. 第二次大范围地移动i退出while循环的结果只有两个：
    1. 到达了字符串的末尾，说明字符串是一个小数，返回true
    2. 遇到了指数符号
11. 若在遇到指数符号之前并没有出现一个完整的小数，即digit和start相等，i和start2相等，则返回false
12. 重复第5步对于指数部分之后的字符进行判断的步骤  
写代码的时候其实采取了函数复用的技巧，对两次大范围转移的步骤进行了函数封装，用变量CASE进行分类，学的是二分法**”寻找一个数的第一个位置和最后一个位置“**那一题  
自己的代码： 
```cpp
class Solution {
private:
    unordered_set<char> set = {'0','1','2','3','4','5','6','7','8','9'};
public:
    bool isInterger(string s,int CASE,int &i) {
        int ls = (int)s.size();
        while (i < ls && ((CASE == 0 && s[i] != '.') || (CASE == 1 && s[i] != 'e' && s[i] != 'E'))) {
            if (set.find(s[i]) != set.end())
                i++;
            else return false;
        }
        return true;
    } 

    bool afterE(string s,int &i) {
        int ls = (int)s.size();
        i++;
        if (i == ls) return false;
        i = (s[i] == '+' || s[i] == '-') ? i + 1 : i;
        if (i == ls) return false;
        while (i < ls && set.find(s[i]) != set.end())
            i++;
        return (i == ls) ? true : false;
    }

    bool isNumber(string s) {
        int ls = (int)s.size();
        int i = (s[0] == '+' || s[0] == '-') ? 1 : 0;
        if (ls == 1 && i == 1) return false;
        int start = i;
        if (!isInterger(s,0,i)) {
            if (s[i] != 'e' && s[i] != 'E' || i == start) return false;
            return (afterE(s,i)) ? true : false;
        }
        if (i == ls) return true;
        int digit = i;
        i++;
        if (i == ls && (i-1) == start) return false;
        else if (i == ls && (i-1) > start) return true;
        int start2 = i;
        if (!isInterger(s,1,i)) return false;
        if (i == ls) return true;
        if (digit == start && i == start2) return false;
        return (afterE(s,i)) ? true : false;
    }
};
```  
看了题解，有关状态机的代码不想放出来了，感觉状态机还未必比我写的简单，但是状态机容易拓展和维护，这一点挺不错的。我看到了有一位水友用很简略的方法直接写出这一题，主要思想是正负号、小数点以及指数部分的出现的先后顺序，这里还是要记录一下的，简直强无敌。  
```cpp
class Solution {
private :
    bool hasNum,hasDigital,hasExp;
public:
    bool isNumber(string s) {
        int ls = (int)s.size();
        for (int i = 0; i < ls; i++) {
            char c = s[i];
            if ((c == '+' || c == '-') && (i == 0 || s[i-1] == 'e' || s[i-1] == 'E')) {
                ;
            } else if ((c == 'e' || c == 'E') && !hasExp && hasNum) {
                hasExp = true;
                hasNum = false;
            } else if (c == '.' && !hasExp && !hasDigital) {
                hasDigital = true;
            } else if (isdigit(c)) {
                hasNum = true;
            } else {
                return false;
            }
        }
        return hasNum;
    }
};
```