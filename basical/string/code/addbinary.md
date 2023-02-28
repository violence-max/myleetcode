题目描述：  
![image](/basical/string/image/image33.png)  
解决过程：  
哎，这道题写了四十多分钟，好久没有简单题能写四十多分钟了，我的脑子感觉被浆糊给灌了。最恶心的地方是自己的思路跟代码逻辑老是对不上，以为完成了自己的思路，实际上对应的代码部分根本没有实现。**我一开始一位字符之间可以直接相加，然后发现并不能，恶心。**  
算法流程：  
1. 创建两个索引，al指向字符串a的尾部，bl指向字符串b的尾部，开始进行循环，当al或者bl超过对应字符串的头部时退出循环
2. 将两个索引所指向的字符进行相加（这里写了一个字符加法函数，可能的结果为’0’、’1’、’2’）。如果结果不为’2’而且进位为’1’，则进行结果和进位的相加，并将进位赋值为’0’；如果结果为’2’而且进位为’1’，则结果赋值为’1’，进位赋值为’1’。这里单独讨论进位是因为我一开始没有找到两个字符和进位直接相加的办法，所以只能两个字符先相加再去讨论进位。
3. 如果结果为’2’则进位为’1’，否则进位不改变
4. 如果结果为’2’则结果更改为’0’，否则不改变
5. 答案字符串的末尾添加结果，两个索引各自减1
6. 这时候两个字符串至少有一个的索引超过了头部，所以单独写了一个函数对没有超过头部的那个字符串的剩余部分做判断
7. 如果索引小于0（超过头部）则返回，否则进入while循环，直至索引超过字符串头部为止退出循环并返回。循环里干的只有一件事：索引指向的字符和进位进行相加，并对结果和进位进行重新赋值然后添加进答案字符串的末尾  
8. 最后处理可可能存在的进位，如果进位为’1’则答案末尾添加’1’，然后反转字符串并且返回答案  
代码：  
```cpp
class Solution {
public:
    void addAlone(string &ans,string str,int index,char &cin) {
        if (index < 0) return;
        while (index >= 0) {
            char c = add(str[index],cin);
            cin = c == '2' ? '1' : '0';
            c = c == '2' ? '0' : c;
            ans += c;
            index--;
        }
    }

    char add(char a,char b) {
        return   (a == '1' && b == '1') ? '2' :
                 (a == '0' && b == '0') ? '0' : '1';
    }

    string addBinary(string a, string b) {
        string ans;
        int la = (int)a.size(),lb = (int)b.size();
        int al = la - 1,bl = lb - 1;
        char cin = '0';
        while (al >= 0 && bl >= 0) {
            char c = add(a[al],b[bl]);
            if (c != '2' && cin == '1') {
                c = add(c,cin);
                cin = '0';
            }
            if (c == '2' && cin == '1') {
                cin = '1';
                c = '1';
            }
            cin = c == '2' ? '1' : (cin == '1') ? cin : cin;
            c = c == '2' ? '0' : c;
            ans += c;
            al--; bl--;
        }
        addAlone(ans,a,al,cin);
        addAlone(ans,b,bl,cin);
        if (cin == '1') ans += '1';
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```  
看了答案发现其实大可不必使用字符进行判断来相加，还有更绝的办法，用int符号来做相加，像做十进制的加法或者乘法一样，每一次相加只将模十的部分添加到答案末尾，然后处于十位的部分作为下一次相加或者相乘的进位  
代码：  
```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        int la = (int)a.size(),lb = (int)b.size();
        reverse(a.begin(),a.end());
        reverse(b.begin(),b.end());
        int n = max(la,lb),cin = 0;
        string ans;
        for (int i = 0; i < n; i++) {
            cin += (i < la) ? (a[i] == '1') : 0;
            cin += (i < lb) ? (b[i] == '1') : 0;
            char c = (cin % 2) ? '1' : '0';
            ans += c;
            cin /= 2;
        }
        if (cin) ans += '1';
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```