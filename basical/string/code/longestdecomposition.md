题目描述：  
![image](/basical/string/code/longestdecomposition.md)  
解决过程：  
没做出来  
- 贪心加双指针：
1. 优先选择长度更小的前缀会使得分割出来的段式回文更多（涉及证明，可以看官方题解复习）
2. 设置左指针left和右指针right，每次迭代len从1开始进行累加尝试：不断尝试以`left+len-1` 和`right-len+1` 开头的长度为len的子串是否相等，如果相等则答案加2，然后立刻退出
3. 在某一次迭代过程中可能会发生子串无法构成前后缀的情况，而这种情况的判断条件为`left + len - 1  ≥ right - len + 1` ，直接答案加1
4. 记得每次尝试得到一个可行的len之后在前缀和后缀分别加上len和减去len
- 滚动哈希：
1. 在第一种解法的基础上先用O(n)的空间复杂度将各个长度的子串预先存储起来的方法，方便对子串的判断  
代码：（贪心+双指针）  
```cpp
class Solution {
public:
    bool judge(const string& text, int l1, int l2, int len) {
        while (len--) {
            if (text[l1++] != text[l2++]) return false;
        }
        return true;
    }

    int longestDecomposition(string text) {
        int left = 0, res = 0, right = text.size() - 1;
        while (left <= right) {
            int len = 1;
            while (left + len - 1 < right - len + 1) {
                if (judge(text,left,right - len + 1,len)) {
                    res += 2;
                    break;
                }
                len++;
            }
            if (left + len - 1 >= right - len + 1) {
                res++;
            }
            left += len;
            right -= len;
        }     
        return res;
    }
};
```  
```cpp
class Solution {
public:
    vector<long long> pre1, pre2;
    vector<long long> pow1, pow2;
    static constexpr int MOD1 = 1000000007;
    static constexpr int MOD2 = 1000000009;
    int Base1, Base2;

    void init(string& s) {
        mt19937 gen{random_device{}()};
        Base1 = uniform_int_distribution<int>(1e6, 1e7)(gen);
        Base2 = uniform_int_distribution<int>(1e6, 1e7)(gen);
        while (Base2 == Base1) {
            Base2 = uniform_int_distribution<int>(1e6, 1e7)(gen);
        }
        int n = s.size();
        pow1.resize(n);
        pow2.resize(n);
        pre1.resize(n + 1);
        pre2.resize(n + 1);
        pow1[0] = pow2[0] = 1;
        pre1[1] = pre2[1] = s[0];
        for (int i = 1; i < n; ++i) {
            pow1[i] = (pow1[i - 1] * Base1) % MOD1;
            pow2[i] = (pow2[i - 1] * Base2) % MOD2;
            pre1[i + 1] = (pre1[i] * Base1 + s[i]) % MOD1;
            pre2[i + 1] = (pre2[i] * Base2 + s[i]) % MOD2;
        }
    }

    pair<int, int> getHash(int l, int r) {
        return {(pre1[r + 1] - ((pre1[l] * pow1[r + 1 - l]) % MOD1) + MOD1) % MOD1, (pre2[r + 1] - ((pre2[l] * pow2[r + 1 - l]) % MOD2) + MOD2) % MOD2};
    }

    int longestDecomposition(string text) {
        init(text);
        int n = text.size();
        int res = 0;
        int l = 0, r = n - 1;
        while (l <= r) {
            int len = 1;
            while (l + len - 1 < r - len + 1) {
                if (getHash(l, l + len - 1) == getHash(r - len + 1, r)) {
                    res += 2;
                    break;
                }
                ++len;
            }
            if (l + len - 1 >= r - len + 1) {
                ++res;
            }
            l += len;
            r -= len;
        }
        return res;
    }
};
```