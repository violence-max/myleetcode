题目描述：

![image](/basical/string/image/image77.png)

解题过程：

没做出来

- 数学+滑动窗口

代码：

```cpp
class Solution {
public:
    bool queryString(string s, int n) {
        if (n == 1) {
            return s.find('1') != -1;
        }
        int k = 30; // n至多只有1e9，因此二进制表示至多30位
        while ((1 << k) > n) {
            --k; // 找到满足n >= 2^k的k的最大值
        }
        int m = s.size();
        if ((m < ((1 << (k - 1)) + k - 1)) || (m < (n - (1 << k) + k + 1))) {
            // 对于[2^{k-1}, 2^k-1]里的数，一共有2^{k-1}个数，并且每个数的长度均为k，
            // 如果长度位m的字符串要包含这些数，至少满足m >= k + 2^{k-1} - 1。k表示
            // 第一个长度为k的数，剩下的2^{k-1} - 1个数对已经存在的数进行复用，每次只
            // 增加一单位长度
            // 对于[2^k,n]里的数同理分析可得
            // 对于[0,2^{k-1}-1]里的数，将[2^{k-1}, 2^k-1]里的最高位的1去掉以后即可得到

            return false;
        }
        return help(s, k, 1 << (k - 1), (1 << k) - 1) && help(s, k + 1, 1 << k, n);
    }

    bool help(const string& s, int k, int mi, int ma) {
        // 判断字符串s中是否存在ma-mi+1个长度为k且互不相同的二进制数

        unordered_set<int> se;
        int m = s.size();
        int t = 0;
        for (int i = 0; i < m; ++i) {
            t = t * 2 + s[i] - '0'; // 从二进制的高位到低位进行循环并且获得一个长度为k的二进制数
            if (i >= k) {
                // 滑动窗口的左侧弹出一个元素

                t -= (s[i - k] - '0') << k;
            }
            if (i >= k - 1 && t <= ma && t >= mi) {
                se.insert(t);
            }
        }
        return se.size() == ma - mi + 1;
    }
};
```