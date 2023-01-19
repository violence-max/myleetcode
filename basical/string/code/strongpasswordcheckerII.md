题目描述：  
![image](/basical/string/image/image43.png)
解决过程：  
1分钟ac  
代码：  
```cpp
class Solution {
public:
    bool strongPasswordCheckerII(string password) {
      int ls = password.length();
      if (ls < 8) return false;
      bool bigletter = false, smallletter = false, digit = false, special = false;
      unordered_set<char> set = {'!','@','#','$','%','^','&','*','(',')','-','+'};
      for (int i = 0; i < ls; i++) {
          if (i < ls - 1 && password[i] == password[i+1]) return false;
          if (!bigletter && 'A' <= password[i]  && password[i] <= 'Z') bigletter = true;
          if (!smallletter && 'a' <= password[i] && password[i] <= 'z') smallletter = true;
          if (!digit && '0' <= password[i] && password[i] <= '9') digit = true;
          if (!special && set.find(password[i]) != set.end()) special = true;
      }  
      return bigletter && smallletter && digit && special;
    }
};
```