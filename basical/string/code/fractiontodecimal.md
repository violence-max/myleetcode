题目描述：  
![image](/basical/string/image/image54.png)  
解决过程：  
很麻烦的一道题目，写了接近两个小时，最后还是通过了，因为觉得自己肯定可以写出来因此就一直在写。写题目的时候我真的吐了，光是想个逻辑都要想半天，改来改去的，以后得着重加强一下自己写之前的思维的能力，要先想好再下笔。这道题目其实最困难的地方应该在于循环小数的处理和溢出的处理，然而我在思考怎么进行除法的模拟上就思考了半天，很不应该，更要命的是我居然没有发现整数的部分是可以直接计算的，还以为需要多次计算。  
关键点：  
1. 哈希map来记录在求小数部分时余数到小数部分下标中的映射
2. 负数的处理
3. 溢出的处理  
题解的方法写得很有条理，因此这里就不记录具体算法流程了。而且题解有个很棒的地方是一开始就判断了是否可以整除，如果可以那就可以提前得到答案，否则必然会出现小数  
代码：（我的→题解）  
```cpp
class Solution {
public:
    string fractionToDecimal(int numerator, int denominator) {
        if (!numerator) return "0";
        bool flag = (numerator > 0) == (denominator > 0);
        long long _numerator = abs((long long)numerator);
        long long _denominator = abs((long long)denominator);
        string decimal, interger;
        bool ifdecimal = false;
        unordered_map<long long,int> origin;
        int index = 0;
        while (_numerator) {
            long long number = _numerator / _denominator, left = _numerator % _denominator;
            if (origin.find(_numerator) != origin.end()) {
                int len2 = index - origin[_numerator];
                int len1 = (decimal.length() > len2) ? decimal.length() - len2 : 0;
                decimal = decimal.substr(0,len1) + '(' + decimal.substr(origin[_numerator],len2) + ')';
                break;
            }
            if (!number) {
                if (!ifdecimal) {
                    ifdecimal = true;
                    interger = interger.length() ? interger : "0";
                } else {
                    decimal += '0';
                    origin[_numerator] = index++;
                }
                _numerator *= 10;
            } else {
                if (!ifdecimal) {
                    interger += to_string(number);
                } else {
                    decimal += to_string(number);
                    origin[_numerator] = index++;
                }
                _numerator = (ifdecimal) ? left * 10 : left;
            }
        }
        string ans = (ifdecimal) ? interger + '.' + decimal : interger;
        return (flag) ? ans : '-' + ans;
    }
};
```
```cpp
class Solution {
public:
    string fractionToDecimal(int numerator, int denominator) {
        long numeratorLong = numerator;
        long denominatorLong = denominator;
        if (numeratorLong % denominatorLong == 0) {
            return to_string(numeratorLong / denominatorLong);
        }

        string ans;
        if (numeratorLong < 0 ^ denominatorLong < 0) {
            ans.push_back('-');
        }

        // 整数部分
        numeratorLong = abs(numeratorLong);
        denominatorLong = abs(denominatorLong);
        long integerPart = numeratorLong / denominatorLong;
        ans += to_string(integerPart);
        ans.push_back('.');

        // 小数部分
        string fractionPart;
        unordered_map<long, int> remainderIndexMap;
        long remainder = numeratorLong % denominatorLong;
        int index = 0;
        while (remainder != 0 && !remainderIndexMap.count(remainder)) {
            remainderIndexMap[remainder] = index;
            remainder *= 10;
            fractionPart += to_string(remainder / denominatorLong);
            remainder %= denominatorLong;
            index++;
        }
        if (remainder != 0) { // 有循环节
            int insertIndex = remainderIndexMap[remainder];
            fractionPart = fractionPart.substr(0,insertIndex) + '(' + fractionPart.substr(insertIndex);
            fractionPart.push_back(')');
        }
        ans += fractionPart;

        return ans;
    }
};
```