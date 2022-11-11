题目描述：  
![image](/basical/string/image/image10.png)    
解题过程：看到觉得是KMP算法，但是我不会，然后我就自己整了一个算法，感觉这个题不是很难啊，可能在时间复杂度优化上有难度吧。我的思想是在找到正确的字符串之前把所有从第一个字符就对得上的地方都比较一遍，代码是精华。草，我把这个东西码出来估计花了四十分钟的样子，但是他居然在一个测试用例里显示超时？然后我换成python代码进行调试，调试完没问题啊！我就感觉很奇怪，但是我的vscode又没有装调试c++的环境，就只能现学gdb和编译，然后用gdb调试了一波（也很曲折），我发现我的一个int值居然定义成了bool类型了，然后回溯的时候`j` 就一直是1.。。。。  
代码：  
```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int i = -1,j = 0,end = (int)needle.size(),length = (int)haystack.size();
        bool exitFlag = false;
        int idx = -1;
        while (j < length) {
            if (haystack[j++] == needle[++i]) {
                if (i == end - 1) return (j - end);
                if (needle[i] == needle[0] && !exitFlag && i != 0) {
                    exitFlag = true;
                    idx = j - 1;
                }
            } else {
                if (i != 0) {
                    if (exitFlag) {
                        j = idx;
                        exitFlag = false;
                    } else {
                        j -= 1;
                    }
                }
                i = -1;
            }
        }
        return -1;
    }
};
```