题目描述：  
![image](/basical/string/image/image27.png)  
 解决过程：  
感觉出题人在挑战我的智商啊。。。  
代码： 
```cpp
class Solution {
public:
    int finalValueAfterOperations(vector<string>& operations) {
        int X = 0;
        for (auto str : operations) {
            if (str == "--X" || str == "X--") {
                X--;
            } else X++;
        }
        return X;
    }
};
```