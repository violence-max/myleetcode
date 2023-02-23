题目描述：  
![image](/basical/IQ/image/image26.png)  
解决过程：
这个题目和格雷编码非常类似，以至于我花了一些时间去复习格雷码的基本解法。我复习好格雷编码之后，思路一直在如果通过 $g_i$来求解$g_{i+1}$但是压根没找到这个方法。题解是这样说的：因为g(0)为0，因此和start异或之后为start，由于$g_i$和$g_{i+1}$也仅有一位不相同，因此分别和start进行异或之后就可以仍然是仅有一位不相同
代码:
```cpp
class Solution {
public:
    vector<int> circularPermutation(int n, int start) {
        int _pow_n = pow(2,n);
        vector<int> res;
        res.reserve(_pow_n);
        for (int i = 0; i < _pow_n; i++) {
            res.push_back((i >> 1) ^ i ^ start);
        }
        return res;
    }
};
```