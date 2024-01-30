#### [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)
+ Dynamic Programming Solution:
  * Time: O(N<sup>2</sup>)
  * Space: O(N<sup>2</sup>)
```c++
class Solution {
public:
    string longestPalindrome(string s) {
        const int N = 1005;
        int dp[N][N];
        memset(dp,0,sizeof dp);
        
        int n = s.size();
        for(int i = 0;i < n;i++){
            dp[i][i] = 1;
        }
        
        int l = 0 , r = 0;
        
        int ans = 1;

        for(int i = 0;i < n - 1;i++){
            if(s[i] == s[i + 1]){
                dp[i][i + 1] = 1;
                ans = 2;
                l = i , r = i + 1;
            }
        }
        for(int len = 3;len <= n;len++){
            for(int i = 0;i + len - 1 < n;i++){
                if(s[i] == s[i + len - 1] and dp[i + 1][i + len - 2] == 1){
                    dp[i][i + len - 1] = 1;
                    ans = len;
                    l = i , r = i + len - 1;
                }
            }
        }
        string str;
        for(int i = l;i <= r;i++)str += s[i];
        return str;
    }
};

```
+ Binary Search + Hashing: 
  - TreeMap
    * Time: O(N)
    * Space: O(N)
  - HashMap
    * Time: O(N)
    * Space: HashMap
+ Manachar algorithm:
  * Time: O(N)


#### [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
+ Two Pointer:
  * Time: O(N)
  * Space: O(Character_Size)
```c++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int cnt1[26],cnt2[26];
        memset(cnt1,0,sizeof cnt1);
        memset(cnt2,0,sizeof cnt2);
        
        for(int i = 0;i < p.size();i++){
            int d = p[i] - 'a';
            cnt1[d]++;
        }
        if(s.size() < p.size())return {};
        
        
        int match = 0;
        for(int i = 0;i < 26;i++){
            if(cnt1[i] == cnt2[i])match++;
        }

        for(int i = 0;i < p.size() - 1;i++){
            int d = s[i] - 'a';
            if(cnt1[d] == cnt2[d])match--;
            cnt2[d]++;
            if(cnt1[d] == cnt2[d])match++;
        }

        int l = 0;
        vector<int>v;
        for(int i = p.size() - 1;i < s.size();i++,l++){
            int d = s[i] - 'a';
            if(cnt1[d] == cnt2[d])match--;
            cnt2[d]++;
            
            if(cnt1[d] == cnt2[d])match++;
            
            if(match == 26)v.push_back(l);
            d = s[l] - 'a';
            
            if(cnt1[d] == cnt2[d])match--;
            cnt2[d]--;
            if(cnt1[d] == cnt2[d])match++;
            
        }
        return v;
    }
};

```
