#### [Evaluate Division](https://leetcode.com/problems/evaluate-division/)
+ BFS: 
  - Time: O(Q * N)
  - Space: O(N)
```c++
class Solution {
public:
    vector<pair<int,double> >g[105];
    
    double bfs(int source,int des){
        double vals[105];
        bool vis[105];
        memset(vis,0,sizeof vis);
        memset(vals,-1,sizeof vals);
        
        queue<int>Q;
        Q.push(source);
        vis[source] = 1;
        vals[source] = 1;
        
        while(!Q.empty()){
            int node = Q.front();
            Q.pop();
            for(int i = 0;i < g[node].size();i++){
                int go = g[node][i].first;
                if(vis[go])continue;
                vis[go] = 1;
                vals[go] = vals[node] * g[node][i].second;
                Q.push(go);
            }
        }
        if(vis[des] == 0)return -1;
        return vals[des];
        
    }
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        map<string,int>mp;
        int id = 0;
        
        for(int i = 0;i < equations.size();i++){
            int a , b;
            if(!mp[equations[i][0]]){
                mp[equations[i][0]] = ++id;
            }
            a = mp[equations[i][0]];
            
            if(!mp[equations[i][1]]){
                mp[equations[i][1]] = ++id;
            }
            b = mp[equations[i][1]];
            
            g[a].push_back(make_pair(b,values[i]));
            if(values[i] != 0)g[b].push_back(make_pair(a,1/values[i]));
            cout << a << " " << b << " " << values[i] << "\n";
        }
        vector<double>ans;
        for(int i = 0;i < queries.size();i++){
            int a , b;
            if(!mp[queries[i][0]] || !mp[queries[i][1]]){
                ans.push_back(-1);
                continue;
            }
            a = mp[queries[i][0]];
            
            b = mp[queries[i][1]];
            ans.push_back(bfs(a,b));


        }
        return ans;
    }
};
```



#### [Word Ladder](https://leetcode.com/problems/word-ladder/)
+ BFS: 
  - Time: O(wordList * stringLen * 26)
  - Space: O(N)
```c++
class Solution {
public:

    unordered_map<string,int>exist,distance;
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        for(string s : wordList)exist[s] = 1;
        
        queue<pair<string,int>>Q;
        Q.push(make_pair(beginWord,1));
        distance[beginWord] = 1;
        
        while(!Q.empty()){
            pair<string,int> P = Q.front();
            Q.pop();
            if(P.first == endWord)return P.second;
            
            for(int i = 0;i < P.first.size();i++){
                for(char ch = 'a';ch <= 'z';ch++){
                    if(ch == P.first[i])continue;
                    string s = P.first;
                    s[i] = ch;
                    if(!exist[s])continue;
                    if(distance[s])continue;
                    distance[s] = P.second + 1;
                    Q.push(make_pair(s,distance[s]));
                }
            }
        }
        return 0;
        
        
    }
};
```
