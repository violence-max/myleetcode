题目描述：  
![image](/basical/string/image/image57.png)  
解决过程：  
直接说题解的算法要点吧：  
1. 从左端开始枚举需要进行替换的子字符串的最小长度（窗口），同时使用一个数组来维护除了窗口之外的四个字母的个数。如果所维护的字母的个数都满足小于等于len / 4，则说明可以通过改变窗口里的子字符串来得到一个平衡字符串
2. 窗口时左闭右开的，原因是每次窗口的右端增加一个元素之后，右指针都要向右再次移动
3. 当一个窗口被确定时，需要固定右指针的位置并且挪动左指针的位置来尝试缩小窗口长度，因此右指针越界不能作为跳出循环的判断依据  
代码：  
```cpp
class Solution {
public:
    int idx(char c) {
        return c - 'A';
    }

    void showmethecnt(vector<int> &cnt) {
        cout << "Q is :" <<cnt[idx('Q')] << ' ';
        cout << "W is :" <<cnt[idx('W')] << ' ';
        cout << "E is :" <<cnt[idx('E')] << ' ';
        cout << "R is :" <<cnt[idx('R')] << ' ';
        cout << endl;
    }

    int balancedString(string s) {
        int len = s.length();
        int standar = len / 4;
        vector<int> cnt (26);
        for (auto &c : s) {
            cnt[idx(c)]++;
        }

        auto check = [&]() {
            if (cnt[idx('Q')] > standar || cnt[idx('W')] > standar || cnt[idx('E')] > standar || cnt[idx('R')] > standar) return false;
            return true;
        };
        if (check()) return 0;
        int res = len;
        for (int left = 0, right = 0; left < len; left++) {
            while (right < len && !check()) {
                cnt[idx(s[right])]--;
                right++;
            }
            if (!check()) break;
            res = min(res, right - left);
            cnt[idx(s[left])]++;
        }
        return res;
    }
};
```