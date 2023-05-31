题目描述：

![image](/basical/IQ/image/image41.png)

解决过程：

做了好久

代码：（我的→题解）

```cpp
class Solution {
public:
    vector<int> string2diff(string& str) {
        int n = str.size();
        vector<int> ans (n - 1);
        for (int i = 1; i < n; i++) {
            ans[i-1] = str[i] - str[i-1];
        }
        return ans;
    }

    void test(const vector<int>& tmp) {
        for (auto i : tmp) {
            cout << i << ' ';
        }
        cout << endl;
    }

    string oddString(vector<string>& words) {
        int n = words[0].size();
        vector<int> origin (n - 1);
        int ans;
        bool diff = false;
        for (int i = 0; i < words.size(); i++) {
            auto tmp = string2diff(words[i]);
            // test(tmp);
            if (i == 0) {
                origin = tmp;
            } else if (diff) {
                ans = origin == tmp ? 1 : 0;
                break;
            } else if (origin != tmp) {
                if (i > 1) {
                    ans = i;
                    break;
                }
                diff = true;
            }
        }
        return words[ans];
    }
};
```

```cpp
class Solution {
public:
    vector<int> get(string &word) {
        vector<int> diff(word.size() - 1);
        for (int i = 0; i + 1 < word.size(); i++) {
            diff[i] = word[i + 1] - word[i];
        }
        return diff;
    }

    string oddString(vector<string>& words) {
        auto diff0 = get(words[0]);
        auto diff1 = get(words[1]);
        if (diff0 == diff1) {
            for (int i = 2; i < words.size(); i++) {
                if (diff0 != get(words[i])) {
                    return words[i];
                }
            }
        }
        return diff0 == get(words[2]) ? words[1] : words[0];
    }
};
```