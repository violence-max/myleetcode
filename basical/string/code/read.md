题目描述：  
![image](/basical/string/image/image48.png)  
解决过程：  
写了挺久的，可能有40分钟感觉，思维都卡在指针的使用上了，而且对于文件的读取逻辑不是很清楚：如果读入的字符个数已经超出了需要读入的字符个数那么需要进行删除操作  
我写的代码很屎山，记录一个code友的做法，很简洁  
代码：（我的→code友）  
```cpp
class Solution {
public:
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    int read(char *buf, int n) {
        int cnt = 0, result = 0;
        char* tail_buf = buf;
        while (n) {
            cnt = read4(tail_buf);
            if (cnt > n) {
                int tmp_n = n;
                while (tmp_n--) tail_buf++;
                int del = cnt - n;
                while (del-- && tail_buf) {
                    char* tmp_tmp = tail_buf++;
                    tmp_tmp = nullptr;
                }
                tail_buf = nullptr;
                cnt = n;
            } else if (cnt < 4) {
                n = cnt;
            }
            result += cnt;
            n -= cnt;
            while (cnt-- && tail_buf) tail_buf++;
        }
        return result;
    }
};
```
```cpp
class Solution {
public:
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    int read(char *buf, int n) {
        int total = 0, current = 0;
        do {
            current = read4(buf + total);
            total += current;
        } while (current == 4 && total < n);
        if (total > n) {
            buf[n] = 0;
            total = n;
        }
        return total;
    }
};
```