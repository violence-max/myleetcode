题目描述：

![image](/basical/array/image/image89.png)

解决过程：

写了40分钟，运用了向后扩展的思想

- 题解剪枝思路：三个部分的二进制如果相等，则每个部分中1的个数都相等，因此arr总和必须是3的倍数才有解；

代码：（暴力→O(n)）

```cpp
class Solution {
public:
    vector<int> threeEqualParts(vector<int>& arr) {
        int n = arr.size();
        int cnt = 0;
        int next[n];
        memset(next, -1, sizeof next);
        int index = n;
        for (int i = n - 1; i >= 0; --i) {
            next[i] = index;
            if (arr[i] == 1) {
                index = i;
            }
        }
        int edge = n / 3;
        int pos = -1;
        for (int i = 0; i < n; ++i) {
            if (pos == -1 && arr[i] == 1) {
                pos = i;
                cnt = 1;
            } else if (pos != -1) {
                cnt++;
            }
            // cout << i << ' ' << cnt << endl;
            if (cnt == 0) {
                continue;
            } else if (cnt > edge) {
                break;
            }

            bool valid = true;
            if (i == 3) {
                // cout << next[i] << ' ' << cnt << endl;
            }
            for (int k = next[i], j = pos; k < next[i] + cnt; ++k, ++j) {
                if (i == 3) {
                    // cout << j << ' ' << k << endl;
                }
                if (arr[j] != arr[k]) {
                    valid = false;
                    break;
                }
            }
            // cout << i << ' ' << cnt << ' ' << next[i] << endl;
            if (valid) {
                int nextindex = next[next[i] + cnt - 1];
                // cout << i << ' ' << nextindex << endl;
                if (n - nextindex != cnt) {
                    continue;
                }
                int valid2 = true;
                for (int q = nextindex, p = pos; q < n; ++q, ++p) {
                    if (arr[q] != arr[p]) {
                        valid2 = false;
                        break;
                    }
                }
                if (valid2) {
                    return {i, next[i] + cnt};
                }
            }
        }

        if (cnt == 0) {
            return {0,2};
        } else {
            return {-1, -1};
        }
    }
};
```

```cpp
class Solution {
public:
    vector<int> threeEqualParts(vector<int>& arr) {
        int sum = accumulate(arr.begin(), arr.end(), 0);
        if (sum % 3 != 0) {
            return {-1, -1};
        }
        if (sum == 0) {
            return {0, 2};
        }

        int partial = sum / 3;
        int first = 0, second = 0, third = 0, cur = 0;
        for (int i = 0; i < arr.size(); i++) {
            if (arr[i] == 1) {
                if (cur == 0) {
                    first = i;
                }
                else if (cur == partial) {
                    second = i;
                }
                else if (cur == 2 * partial) {
                    third = i;
                }
                cur++;
            }
        }

        int len = (int)arr.size() - third;
        if (first + len <= second && second + len <= third) {
            int i = 0;
            while (third + i < arr.size()) {
                if (arr[first + i] != arr[second + i] || arr[first + i] != arr[third + i]) {
                    return {-1, -1};
                }
                i++;
            }
            return {first + len - 1, second + len};
        }
        return {-1, -1};
    }
};
```