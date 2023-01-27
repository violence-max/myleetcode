题目描述：  
![image](/basical/string/image/image49.png)  
解决过程：  
这道题和1的最大区别是read会被多次调用，也就是说，当n比较大时，文件里可能会有一部分字符串被“过分”地读取到目标缓存区中，因此要额外设置一个缓存区来存储这部分字符（有个code友用一个大小为4的缓存区把字符串的正常读取和存储被额外读取的部分文件统一起来了）  
代码：（我的→code友）  
```cpp
class Solution {
private:
    char leftbuf[512];
    int end = 0;
public:
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    int read(char *buf, int n) {
        int total = 0, current = 0;
        if (end > 0) {
            for (int i = 0; i < end; i++) {
                *(buf + i) = leftbuf[i];
                leftbuf[i] = 0;
            }
            total += end;
        }
        do {
            current = read4(buf + total);
            total += current;
        } while (current == 4 && total < n);
        if (total > n) {
            end = total - n;
            for (int i = 0; i < end; i++) {
               leftbuf[i] = *(buf + n + i);
            }
            buf[n] = 0;
            total = n;
        }
        return total;
    }
};
```
```cpp
class Solution {
private:
    char leftbuf[4];
    int bufsize = 0;
    int bufindex = 0;
public:
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    int read(char *buf, int n) {
        int index = 0;
        while (index < n) {
            while (bufindex < bufsize && index < n) {
                buf[index++] = leftbuf[bufindex++];
            }
            if (index < n) {
                bufsize = read4(leftbuf);
                bufindex = 0;
                if (bufsize == 0) break;
            }
        }
        return index;
    }
};
```