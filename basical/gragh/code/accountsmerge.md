题目描述：

![image](/basical/gragh/image/image27.png)

解决过程：

没有做出来，并查集， 考验码力

代码：

```sql
class uf {
public:
    vector<int> parent;
    uf(int n) {
        parent.resize(n);
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }

    void merge(int index1, int index2) {
        parent[find(index2)] = find(index1);
    }

    int find(int index) {
        if (parent[index] != index) {
            return parent[index] = find(parent[index]);
        }
        return index;
    }
};

class Solution {
public:
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        unordered_map<string, int> emailToIndex;
        unordered_map<string, string> emailToName;
        int index = 0;
        for (auto& ac : accounts) {
            string name = ac[0];
            int n = ac.size();
            for (int i = 1; i < n; ++i) {
                emailToIndex[ac[i]] = index++;
                emailToName[ac[i]] = name;
            }
        }
        uf uf_ = uf(index);
        for (auto& ac : accounts) {
            int n = ac.size();
            int firstindex = emailToIndex[ac[1]];
            for (int i = 2; i < n; ++i) {
                uf_.merge(firstindex, emailToIndex[ac[i]]);
            }
        } 
        unordered_map<int, vector<string>> indexToEmails;
        for (auto& [email, index] : emailToIndex) {
            int p = uf_.find(index);
            indexToEmails[p].push_back(email);
        }
        vector<vector<string>> ret;
        for (auto& [index, emails] : indexToEmails) {
            sort(emails.begin(), emails.end());
            vector<string> account;
            account.push_back(emailToName[emails[0]]);
            for (auto email : emails) {
                account.push_back(email);
            }
            ret.push_back(move(account));
        }
        return ret;
    }
};
```