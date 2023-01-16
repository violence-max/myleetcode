题目描述：  
![image](/basical/string/code/aresentencesimilar.md)  
解决过程：  
想得稍微有一点复杂。一开始肯定是通过空格分隔字符串得到一个单词数组没问题了，但是我的想法一头栽倒了怎么通过模拟插入的方式来判断两个句子是否相似而忽略了特性的统一，没有去思考一种简洁的可以把所有情况总结起来的方法得到一段短小精悍的代码而是选择了疯狂分类讨论，哎，这种统一的tricky的代码技巧还是要多磨练。  
我的思路：设s1为句子1的单词数组，s2为句子2的单词数组。假设可以在s1中插入句子得到s2，则如果s1的长度大于s2那肯定不可能；如果s1的长度等于s2则两者必须相同；如果s1的长度小于s2则s1必须为s2的头一端的子集或者s1为s2的尾一端的子集或者s1的前半部分为s2的头一端的子集且s1的后半部分为s2的尾一端的子集，s1和s2才相似，否则都不相似  
题解的思路就很简洁：i表示从左往右开始s1和s2的相等的单词的个数，j表示从右往左开始s1和s2单词相等的个数，如果i+j刚好是s1和s2中长度最小的那个的长度那么说明句子1和句子2相似。但是啊，这样的写法不一定时间最优，因为每次都要遍历O(n+m)次，然而我的写法是可以提前判断多种情况一定不可能相似的。各有利弊吧（测试之后证明我的0ms完胜题解）  
代码（我的→题解）  
```cpp
class Solution {
public:
    vector<string> getsentence(const string& sentence) {
        int ls = sentence.length(), i = 0;
        vector<string> ans;
        while (i < ls) {
            string tmp;
            int start = i;
            while (i < ls && sentence[i] != ' ')
                i++;
            tmp = sentence.substr(start,i - start);
            ans.push_back(tmp);
            i++;
        }
        return ans;
    }

    bool check(const vector<string>& s1,const vector<string>& s2,int start1,int start2,int length) {
        for (int i1 = start1,i2 = start2; i1 < start1 + length && i2 < start2 + length; i1++,i2++) {
            if (s1[i1] != s2[i2]) {
                return false;
            }
        }
        return true;
    }

    bool similar(const vector<string>& s1,const vector<string>& s2) {
        int m = s1.size(), n = s2.size();
        if (m == n) {
            return check(s1,s2,0,0,m);
        } else if (m > n) {
            return false;
        } else {
            if (m == 1) return s1[0] == s2[0] || s1[m-1] == s2[n-1];
            if (s1[0] != s2[0] && s1[m-1] != s2[n-1]) {
                return false;
            } else if (s1[0] != s2[0]) {
                return check(s1,s2,0,n-m,m);
            } else if (s1[m-1] != s2[n-1]) {
                return check(s1,s2,0,0,m);
            } else {
                for (int i = 1; i < m; i++) {
                    if (check(s1,s2,0,0,i) && check(s1,s2,i,n-m+i,m-i)) {
                        return true;
                    }
                }
                return false;
            }
        }
    }

    bool areSentencesSimilar(string sentence1, string sentence2) {
        vector<string> s1 = getsentence(sentence1), s2 = getsentence(sentence2);
        return similar(s1,s2) || similar(s2,s1);
    }
};
```  
```cpp
class Solution {
public:
    vector<string_view> split(const string & str, char target) {
        vector<string_view> res;
        string_view s(str);
        int pos = 0;
        while (pos < s.size()) {
            while (pos < s.size() && s[pos] == target) {
                pos++;
            }
            int start = pos;
            while (pos < s.size() && s[pos] != target) {
                pos++;
            }
            if (pos > start) {
                res.emplace_back(s.substr(start, pos - start));
            }
        }
        return res;
    }

    bool areSentencesSimilar(string sentence1, string sentence2) {
        vector<string_view> words1 = split(sentence1, ' ');
        vector<string_view> words2 = split(sentence2, ' ');
        int i = 0, j = 0;
        while (i < words1.size() && i < words2.size() && words1[i] == words2[i]) {
            i++;
        }
        while (j < words1.size() - i && j < words2.size() - i && words1[words1.size() - j - 1] == (words2[words2.size() - j - 1])) {
            j++;
        }
        return i + j == min(words1.size(), words2.size());
    }
};
```