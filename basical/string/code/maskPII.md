题目描述：  
![image](/basical/string/image/image66.png)  
解题过程：  
写了20分钟，一堆屎山，来看看题解吧：  
1. 直接在字符串中寻找字符’@’，如果找到了就执行邮件的隐藏逻辑：使用transform来和tolower函数来对字符串s中的所有大写字母进行转变，最后取s的第一个字符，中间加上星号，后缀是字符’@’之后的字符串
2. 执行隐藏电话的逻辑：注意到前缀只有0位到4位是不同的，又和电话号码中的数字个数相关，因此可以用将除了数字0~9以外的字符剔除了以后得到的字符串的长度n与10做差得到的值进行对前缀的索引
3. 正则表达式：[^0-9]表示出了数字0~9以外的字符，将其替换为””即可快速得到预期字符串  
代码：（屎山→题解）  
```cpp
class Solution {
public:
    string hideEmail(const string& s) {
        int aiteIdex = -1;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '@') aiteIdex = i; 
        }
        string prev = BigLetter2SmallLetter(s.substr(0,aiteIdex));
        // prev = prev[0] + prev[prev.size()-1];
        char first = prev[0], last = prev[prev.size()-1];
        string pre;
        pre += first;
        pre += "*****";
        pre += last;
        // cout << first << ' ' << last << ' ' << pre;
        return pre + '@' + BigLetter2SmallLetter(s.substr(aiteIdex+1));

    }

    string BigLetter2SmallLetter(const string& str) {
        string ans;
        for (auto s : str) {
            if ('A' <= s && s <= 'Z') {
                s = s + 32;
            }
            ans += s;
        }
        return ans;
    }

    string hidePhoneNumber(const string& str) {
        string phoneNumber;
        for (int i = 0; i < str.size(); i++) {
            if (str[i] == '+' || str[i] == '-' || str[i] == '(' || str[i] == ')' || str[i] == ' ') continue;
            phoneNumber += str[i];
        }
        int n = phoneNumber.size();
        string sfuffix = phoneNumber.substr(n-4), res;
        switch (n) {
            case 10 :
                res = "***-***-";
                break;
            case 11 :
                res = "+*-***-***-";
                break;
            case 12 :
                res = "+**-***-***-";
                break;
            default :
                res = "+***-***-***-";
        }
        res += sfuffix;
        return res;
    }

    string maskPII(string s) {
        if (isalpha(s[0])) {
            return hideEmail(s);
        } else {
            return hidePhoneNumber(s);
        }
    }
};
```  
```cpp
class Solution {
public:
    vector<string> country = {"", "+*-", "+**-", "+***-"};

    string maskPII(string s) {
        string res;
        int at = s.find("@");
        if (at != string::npos) {
            transform(s.begin(), s.end(), s.begin(), ::tolower);
            return s.substr(0, 1) + "*****" + s.substr(at - 1);
        }
        s = regex_replace(s, regex("[^0-9]"), "");
        return country[s.size() - 10] + "***-***-" + s.substr(s.size() - 4);
    }
};
```