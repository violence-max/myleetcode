题目描述：  
![image](/basical/IQ/image/image9.png)  
解题过程：  
很明显的下一个排列嘛，15分钟直接ac了。但是在重新书写下一个排列的过程中发生了错误，本来想从后往前找到第一个不满足单调减的数字的序号的，但是找到了最后一个单调减的序号（忘记减一了）。题目通过了之后重新看了下一个排列那道题，发现自己很侥幸，因为下一个排列那题是可以实现自循环的，而这个题刚好不会出现最大的排列继续寻找下一个排列的情况，因为k ≤ n!。而且我以为在交换了之后需要对那坨单调减的数字进行升序排序，忘记了可以直接倒转了。  
代码：  
```cpp
class Solution {
public:
    string generateString(vector<int> nums) {
        string str;
        for (auto i : nums) {
            str += to_string(i);
        }
        return str;
    }
    void nextPermutation(vector<int> &v) {
        int m = v.size();
        int i = m - 1;
        while (i > 0 && v[i] <= v[i-1])
            i--;
        i--;
        if (i >= 0) {
            int j = m - 1;
            while (j > i && v[j] <= v[i])
                j--;
            swap(v[i],v[j]);
            reverse(v.begin()+i+1,v.end());
        }
    } 

    string getPermutation(int n, int k) {
        vector<int> v;
        int cur = 1;
        while (cur <= n) {
            v.push_back(cur++);
        }
        while (--k) {
            nextPermutation(v);
        }
        return generateString(v);
    }
};
```  
官解：  
采用逐位进行选取的方法减少问题的规模，学到了整数转化为字符串的一个有趣的方法：（j + ‘0’）。官解写得挺精髓的，包括向上取整的写法，还有排序变量的定义，和选取哪个数作为新的末尾，有效数组的赋值和运用。实际扯原理就可以写很多了，不扯那么多了，代码写得很清楚了。  
```cpp
class Solution {
public:
    string getPermutation(int n, int k) {
        vector<int> factorial(n);
        factorial[0] = 1;
        for (int i = 1; i < n; ++i) {
            factorial[i] = factorial[i - 1] * i;
        }

        --k;
        string ans;
        vector<int> valid(n + 1, 1);
        for (int i = 1; i <= n; ++i) {
            int order = k / factorial[n - i] + 1;
            for (int j = 1; j <= n; ++j) {
                order -= valid[j];
                if (!order) {
                    ans += (j + '0');
                    valid[j] = 0;
                    break;
                }
            }
            k %= factorial[n - i];
        }   
        return ans;     
    }
};
```