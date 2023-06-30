题目描述：

![image](/basical/string/image/image81.png)

解决过程：

2分钟ac

代码：

```cpp
class Solution {
public:
    bool isCircularSentence(string sentence) {
        if (sentence.back() != sentence[0]) {
            return false;
        }
        int i = 0;
        int n = sentence.size();
        while (i < n) {
            int j = i;
            while (j < n && sentence[j] != ' ') {
                j++;
            }
            if (j == n) {
                break;
            }
            if (sentence[j-1] != sentence[j+1]) {
                return false;
            }
            i = j + 1;
        }
        return true;
    }
};
```