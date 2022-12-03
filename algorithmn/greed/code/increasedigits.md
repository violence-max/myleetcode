题目描述： 
![image](/algorithmn/greed/image/image15.png)  
解题过程： 
这个题思考时间没多久，实现时间挺久的，尤其是在自己不断检测自己代码能否通过例子的时候，不断地发现漏洞然后去把这个部分给完善并且实现。最要命的一个例子 1234居然没有通过？我检查了一遍自己的代码发现应该没有问题，然后gdb，gdb答案是正确。在我几次提交结果不断更改的情况下我推测可能是某个值没有初始化，于是我怀疑起了自己设置的返回变量—ans，然后把它改成了0，结果通过了。我真的无语了。我终于发现在一个本来就是单调递增的数字的情况下某些c++编译器会给ans赋一个很奇怪的值，而我使用的gdb会直接赋值为0。离谱。  
看了题解，发现转字符串的方法可以从高位到低位遍历，妙啊，这里代码安排上。  
代码：  
```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        int num = n % 10;
        n = n / 10;
        if (n == 0) return num;
        int nextnum = n % 10;
        int ans = 0;
        int record = 0;
        int prenum = -1;
        while (n) {
            int temp;
            int tmpRecord = record;
            if (nextnum > num) {
                temp = 9;
                if (prenum == -1) {
                    int sum = 0;
                    for (int i = 0; i < tmpRecord; i++) {
                        sum = sum * 10 + 9;
                    }
                    ans = sum;
                }
                prenum = 9;
                num = nextnum - 1;
            } else {
                temp = num;
                num = nextnum;
                prenum = -1;
            }
            if (record > 0) {
                while (tmpRecord--) temp *= 10;
            }
            ans += temp;
            record++;
            n = n / 10;
            if (n == 0) {
                if (num == 0) break;
                while (record--) {
                    num *= 10;
                }
                ans += num;
                break;
            }
            nextnum = n % 10;
        }
        return ans;
    }
};
```  
高位向低位遍历，使用stoi和to_string的技巧  
```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string s = to_string(n);
        int i = 1;
        while (i < s.size() && s[i-1] <= s[i])
            i++;
        if (i < s.size()) {
            while (i > 0 && s[i-1] > s[i])
                s[(i--)-1]--;
            for (i += 1; i < s.size(); i++)
                s[i] = '9';
        }
        return stoi(s);
    }
};
```