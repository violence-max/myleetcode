题目描述：  
![Untitled](/basical/string/image/image44.png)  
解题过程：  
sb题目，这么费时间，coding了一个多小时，提交失败了整整9次才结束这该死的题目，由于我是用模拟的方法来做的因此有n种情况我没有考虑到，难受啊。  
算法流程：（模拟）  
1. 用valid数组（大小为字符串的长度，初值赋为全true）记录字符串对应字符是否应该出现在答案种，i赋初值为1进行循环，i越过字符串末尾则退出
2. 若第i位字符为’.’且第i-1位字符为’/’则继续判断：若第i+1位字符位’.’（若存在），则用start记录i，end记录i+2。若end小len且第end位字符位‘/’或者end等于len则说明遇到了”../”，进行字符串“回溯”（将一个范围内的字符设置位false）：向前找到最后一个’/’，往后找到第二个’/’，将两个斜杠范围内的字符设置位false，i赋值为end；否则用start记录i，用end记录i+1，若第end位字符为‘/’或者end等于len则说明遇到了“./”，则进行范围内的置0：end向前找到最后一个连续的’/’为止，将start至end范围内的字符置2为false，i赋值为end
3. 若第i位字符为’/’且为最后一位字符则置为false
4. 若dii位字符位’/’且第i+1位字符也为’/’则说明遇到了连续的斜杠，此时若i等于1则start记录i否则start记录i+1，end记录i+1，end向前找到最后一个连续的‘/’，将start至end范围内的字符置为false，i赋值为end
5. i向前移动一位
6. 等字符串遍历结束以后，将valid数组对应下标为true的字符逐一添加到答案数组中即可。但是，用这种模拟的方法得到的字符串可能存在漏洞：由于最后一个路径为”./”导致上一个路径最后位置的斜杠添加到答案中，而题目要求最后一个路径不能有斜杠，因此在生成答案的时候注意判断  
代码：  
```cpp
class Solution {
public:
    void markFalse(vector<bool> &valid,int start,int end) {
        for (int i = start; i <= end; i++) {
            valid[i] = false;
        }
    }

    string generate(vector<bool> valid,string path) {
        string ans;
        for (int i = 0; i < path.length(); i++) {
            if (valid[i]) {
                ans += path[i];
            }
        }
        int len = ans.length();
        return (ans[len-1] == '/' && len != 1) ? ans.substr(0,ans.length()-1) : ans;
    }

    string simplifyPath(string path) {
        int len = path.length();
        vector<bool> valid (len,true);
        int i = 1;
        while (i < len) {
            if (path[i] == '.' && path[i-1] == '/') {
                if (i+1 < len && path[i+1] == '.') {
                    int end = i + 2;
                    if ((end < len && path[end] == '/') || end == len) {
                        while (end < len && path[end] == '/')
                            end++;
                        end = (path[end-1] == '/') ? end - 1 : end;
                        int start = i - 1,cnt = 0;
                        while (start >= 1) {
                            if (path[start] == '/' && valid[start]) {
                                if (cnt == 1) {
                                    start++;
                                    break;
                                }
                                else cnt++;
                            }
                            start--;
                        }
                        start = (start == 0) ? 1 : start;
                        markFalse(valid,start,end);
                    }
                    i = end;
                } else {
                    int start = i;
                    int end = i + 1;
                    if ((end < len && path[end] == '/') || end == len) {
                        while (end < len && path[end] == '/')
                            end++;
                        end = (end != 1 && path[end-1] == '/') ? end - 1 : end;
                        markFalse(valid,start,end);
                    }
                    i = end;
                }
            } else if (i == len - 1 && path[i] == '/') valid[i] = false;
            else if (path[i] == '/' && path[i+1] == '/') {
                int start = (i == 1) ? i : i + 1,end = i + 1;
                while (end < len && path[end] == '/')
                    end++;
                end -= 1;
                markFalse(valid,start,end);
                i = end;
            }
            i++;
        }
        return generate(valid,path);
    }
};
```  
题解用栈去模拟，比我这个思路强一百倍。先以分隔符’/’把斜杠内的路径名称全部取出来放到一个容器里，然后将这这个容器里的名称逐一入账，而入栈规则为：若遇到了”..”则将栈弹出一个元素（若栈非空），否则将不为”.”的元素压入栈中。如栈为空则返回单斜杠，否则用斜杠逐一拼接栈中的元素得到答案  
代码：  
```cpp
class Solution {
public:
    string simplifyPath(string path) {
        auto split = [](const string& s, char delim) -> vector<string> {
            vector<string> ans;
            string cur;
            for (char ch: s) {
                if (ch == delim) {
                    ans.push_back(move(cur));
                    cur.clear();
                }
                else {
                    cur += ch;
                }
            }
            ans.push_back(move(cur));
            return ans;
        };

        vector<string> names = split(path, '/');
        vector<string> stack;
        for (string& name: names) {
            if (name == "..") {
                if (!stack.empty()) {
                    stack.pop_back();
                }
            }
            else if (!name.empty() && name != ".") {
                stack.push_back(move(name));
            }
        }
        string ans;
        if (stack.empty()) {
            ans = "/";
        }
        else {
            for (string& name: stack) {
                ans += "/" + move(name);
            }
        }
        return ans;
    }
};
```