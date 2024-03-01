题目描述：

![image](/basical/IQ/image/image42.png)

解决过程：

没做出来

- 基于进制数相加的数学推导

代码：

```cpp
class Solution {
public:
    vector<int> addNegabinary(vector<int>& arr1, vector<int>& arr2) {
        int i = arr1.size() - 1, j = arr2.size() - 1;
        int carry = 0;
        vector<int> ans;
        while (i >= 0 || j >= 0 || carry != 0) {
            // x = arr[i] + arr[j] + carry
            int x = carry;
            if (i >= 0) {
                x += arr1[i];
            }
            if (j >= 0) {
                x += arr2[j];
            }

            if (x >= 2) {
                // (-2)^i + (-2)^i = -(-2)^{i+1}，所以进位为-1
                ans.push_back(x - 2);
                carry = -1;
            } else if (x >= 0) {
                ans.push_back(x);
                carry = 0;
            } else {
                // 进位为-1且两位之和为0时，x = -1
                // 设具有进位-1的位数为第k位，则由于最终结果中只有{0,1}，可得：
                // -(-2)^k = (-2)^k + (-2)^{k+1}
                // -(-2)^k: 当前进位为-1; (-2)^k: -1要转成绝对值1; (-2)^{k+1}: 最终结果为第k+1位取1，即具有进位1

                ans.push_back(1);
                carry = 1;
            }

            --i;
            --j;
        }
        while (ans.size() > 1 && ans.back() == 0) {
            // 去除前导0

            ans.pop_back();
        }
        reverse(ans.begin(), ans.end());

        return ans;
    }
};
```