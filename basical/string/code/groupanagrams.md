题目描述：
![image](/basical/string/image/image26.png)
解题过程： 
思路非常简单的一个题目，一开始我想着用多个集合去容纳字母异位词分组，直到我遇到了两个空串的测试用例然后我发现如果出现两个一样的字母我就寄了。于是我把集合里的string类型换成了装有string的容器vector类型，然而这样子也根本不行。于是我发现，既然我的代码里面基本上没有用到有关集合的快速查找性质，那我直接把集合砍掉，换成一个容器就好了，这样子可以有效地加入多个相同的字母。结果，超时了。。。 
这里还有个好笑的地方，我一直以为测试用例[””,””]是字符串”,”的意思，后面才发现是两个空字符串，那个逗号是分隔符。  
题解：  
题解提供了两种方法，一种是排序，另一种是计数。计数的方法用到了一些c++的高级语法特性，没有去深究。排序的方法就很牛逼了，直接把字符串中的字符按照字典序进行排序，然后以排序后的字符串为键，值为一个装有string类型的容器，强无敌。 
代码： 
排序：  
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> map;
        for (string str : strs) {
            string k = str;
            sort(k.begin(),k.end());
            map[k].push_back(str);
        }
        vector<vector<string>> ans;
        for (auto it = map.begin(); it != map.end(); it++) {
            ans.emplace_back(it->second);
        }
        return ans;
    }
};
```  
超时： 
```cpp
class Solution {
public:
    bool ismatched(string s,vector<string> &nowset) {
        string str = nowset[0];
        unordered_map<char,int> letter;
        for (char c : str) {
            letter[c]++;
        }
        for (char c : s) {
            letter[c]--;
        } 
        for (auto [_,freq] : letter) {
            if (freq != 0) return false;
        }
        nowset.push_back(s);
        return true;
    }

    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> set;
        for (string s : strs) {
            bool insertFlag = false;
            for (auto &nowset : set) {
                if (ismatched(s,nowset)) {
                    insertFlag = true;
                    break;
                }
            }
            if (!insertFlag) {
                set.push_back({s});
            }
        }
        vector<vector<string>> ans;
        for (auto realset : set) {
            ans.emplace_back(realset);
        }
        return ans;
    }
};
``` 
计数： 
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        // 自定义对 array<int, 26> 类型的哈希函数
        auto arrayHash = [fn = hash<int>{}] (const array<int, 26>& arr) -> size_t {
            return accumulate(arr.begin(), arr.end(), 0u, [&](size_t acc, int num) {
                return (acc << 1) ^ fn(num);
            });
        };

        unordered_map<array<int, 26>, vector<string>, decltype(arrayHash)> mp(0, arrayHash);
        for (string& str: strs) {
            array<int, 26> counts{};
            int length = str.length();
            for (int i = 0; i < length; ++i) {
                counts[str[i] - 'a'] ++;
            }
            mp[counts].emplace_back(str);
        }
        vector<vector<string>> ans;
        for (auto it = mp.begin(); it != mp.end(); ++it) {
            ans.emplace_back(it->second);
        }
        return ans;
    }
};
```