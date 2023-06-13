题目描述：

![image](/basical/string/image/image78.png)

解决过程：

写了一个多小时，暴力写出来了，然后题解告诉我元音的部分还可以优化。。

代码：

```cpp
class Solution {
public:
    unordered_set<char> set = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};

    string tolower(const string& str) {
        int n = str.size();
        string ret;
        for (int i = 0; i < n; ++i) {
            ret += str[i];
            if (ret[i] >= 'A' && ret[i] <= 'Z') {
                ret[i] += 32;
            }
        }
        return ret;
    }

    string devow(const string& str) {
        int n = str.size();
        string ret;
        for (int i = 0; i < n; ++i) {
            ret += set.count(str[i]) ? '*' : str[i] >= 'A' && str[i] <= 'Z' ? str[i] + 32 : str[i];
        }
        return ret;
    }

    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        int n = queries.size(), m = wordlist.size();
        unordered_set<string> dic;
        unordered_map<string, string> lowtoany;
        unordered_map<string, string> lowtoaeiou;
        for (auto& word : wordlist) {
            dic.insert(word);
            string lword = tolower(word);
            if (!lowtoany.count(lword)) {
                lowtoany[lword] = word;
            }
            string deword = devow(word);
            if (!lowtoaeiou.count(deword)) {
                lowtoaeiou[deword] = word;
            }
        }
        Debug(lowtoaeiou);
        vector<string> ans (n);
        for (int i = 0; i < n; ++i) {
            string tmp = queries[i];
            string tmp2 = tolower(tmp);
            string tmp3 = devow(tmp);
            ans[i] = dic.count(tmp) ? tmp : lowtoany.count(tmp2) ? lowtoany[tmp2] : lowtoaeiou.count(tmp3) ? lowtoaeiou[tmp3] : "";
        }
        return ans;
    }

    void Debug(const unordered_map<string, string>& map) {
        for (auto [u, v] : map) {
            cout << u << ' ' << v << endl;
        }
    }
};
```