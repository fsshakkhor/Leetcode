#### [K-Concatenation Maximum Sum](https://leetcode.com/problems/k-concatenation-maximum-sum/)
+ Dynamic Programming: 
  - Time: O(N)
  - Space: O(1)
  
```c++
class Solution {
public:
    
    long long max(long long a,long long b){
        if(a > b)return a;
        else return b;
    }
    int kConcatenationMaxSum(vector<int>& arr, int k) {
        const long long mod = 1e9 + 7;
        long long sa = 0 , pref = 0, suff = 0;
        
        long sum = 0,running_sum = 0;
        for(int i = 0;i < arr.size();i++){
            sum += arr[i];
            running_sum += arr[i];
            
            if(running_sum < 0)running_sum = 0;
            
            sa = max(sa,running_sum);
            pref = max(pref,sum);
        }
        running_sum = 0;
        for(int i = arr.size() - 1;i >= 0;i--){
            running_sum += arr[i];
            suff = max(suff,running_sum);
        }
        long long ans = 0;

        if(k == 1)ans = sa;
        else if(k == 2)ans = max(sa,suff + pref);
        else{
            ans = max(ans,max(sa,(k - 2) * max(0LL,sum) + suff + pref));
        }
        return ans % mod;
    }
};
```

#### [Count Square Submatrices with All Ones](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)
+ Dynamic Programming: 
  - Time: O(N<sup>2</sup>)
  - Space: O(N<sup>2</sup>)
  
```c++
class Solution {
public:
    int countSquares(vector<vector<int>>& matrix) {
        int n = matrix.size();
        int m = matrix[0].size();
        int dp[n][m];
        memset(dp,0,sizeof dp);
        long long ans = 0;
        for(int i = 0;i < n;i++){
            for(int j = 0;j < m;j++){
                if(matrix[i][j] == 0)continue;
                int pre = 0;
                if(i > 0 and j > 0){
                    pre = min(dp[i - 1][j],min(dp[i - 1][j - 1],dp[i][j - 1]));
                }
                dp[i][j] = 1 + pre;
                ans += dp[i][j];
            }
        }
        return ans;
    }
};
```

#### [Jump Game II](https://leetcode.com/problems/jump-game-ii/)
+ Dynamic Prgramming: 
  - Time: O(N)
  - Space: O(N)
  
```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        const int N = 30005;
        int dp[N];
        fill(dp,dp + N,-1);
        dp[0] = 0;
        
        int n = nums.size();
        int done = 0,curlev = 0;
        for(int i = 0;i < nums.size();i++){
            curlev = dp[i];
            int go = min(n - 1,i + nums[i]);
            while(done < go){
                done++;
                dp[done] = curlev + 1;
            }
        }
        return dp[nums.size() - 1];        
    }
};

```

#### [Guess Number Higher or Lower II](https://leetcode.com/problems/guess-number-higher-or-lower-ii/)
+ Dynamic Prgramming: 
  - Time: O(N<sup>3</sup>)
  - Space: O(N<sup>2</sup>)

- Recursive Implementation
```c++
class Solution {
public:

    int dp[205][205];
    int call(int l,int r){
        if(l > r)return 0;
        if(l == r)return 0;
        if(dp[l][r] != -1)return dp[l][r];
        int ret = 1e9;
        for(int i = l;i <= r;i++){
            int cost = i + max(call(l,i - 1),call(i + 1,r));
            ret = min(ret,cost);
        }
        return dp[l][r] = ret;
    }
    int getMoneyAmount(int n) {
        memset(dp,-1,sizeof dp);
        return call(1,n);
    }
};

```

- Iterative Implementation
```c++
class Solution {
public:

    int dp[205][205];
    int getMoneyAmount(int n) {
        for(int len = 2;len <= n;len++){
            for(int i = 1,j = i + len - 1;j<= n;i++,j++){
                int ret = 1e9;
                for(int mid = i;mid <= j;mid++){
                    int cost = max(dp[i][mid - 1],dp[mid + 1][j]) + mid;
                    ret = min(ret,cost);
                }
                dp[i][j] = ret;
            }
        }
        return dp[1][n];
    }
};

```

#### [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)
+ Dynamic Prgramming: 
  - Time: O(N<sup>2</sup>)
  - Space: O(N<sup>2</sup>)
  
```c++
class Solution {
public:
    int numDistinct(string s, string t) {
        int n = s.size();
        int m = t.size();
        long long dp[n + 1][m + 1];
        memset(dp,0,sizeof dp);
        
        for(int i = 0;i <= n;i++){
            for(int j = 0;j <= m;j++){
                if(j == 0)dp[i][j] = 1;
                else if(i == 0)dp[i][j] = 0;
                else{
                    if(s[i-1] == t[j - 1])dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
                    else dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[n][m];
    }
};

```

#### [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)
+ Dynamic Prgramming: 
  - Time: O(N<sup>2</sup>)
  - Space: O(N<sup>2</sup>)
  
```c++
class Solution {
public:
    int minCut(string s) {
        int n = s.size();
        
        int pal[n][n] , dp[n + 1];
        memset(pal,0,sizeof pal);
        
        for(int i = 0;i < n;i++)pal[i][i] = 1 , dp[i] = 1e9;
        for(int i = 0;i + 1 < n;i++)if(s[i] == s[i + 1])pal[i][i + 1] = 1;
        
        for(int len = 3;len <= n;len++){
            for(int i = 0;i + len - 1 < n;i++){
                if(s[i] == s[i + len - 1] and pal[i + 1][i + len - 2]){
                    pal[i][i + len - 1] = 1;
                }
            }
        }
        
        dp[n] = 0;
        for(int i = n - 1;i >= 0;i--){
            for(int j = i;j < n;j++){
                if(pal[i][j])dp[i] = min(dp[i],1 + dp[j + 1]);
            }
        }
        return dp[0] - 1;
        
    }
};

```

#### [Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)
+ Dynamic Prgramming: 
  - Time: O(N<sup>2</sup>)
  - Space: O(N<sup>2</sup>)
  
```c++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n = s.size();
        int m = p.size();
        
        
        bool dp[n + 1][m + 1];
        
        bool prev[n + 1];
        memset(dp,false,sizeof dp);
        memset(prev,false,sizeof prev);
        
        for(int j = 0;j <= m;j++){
            
            for(int i = 0;i <= n;i++){
                if(i == 0 and j == 0)dp[i][j] = true;
                else if(j == 0)dp[i][j] = false;
                else if(i == 0){
                    if(dp[i][j - 1] and p[j - 1] == '*')dp[i][j] = true;
                }else{
                    if(p[j - 1] == '?')dp[i][j] = dp[i - 1][j - 1];
                    else if(p[j - 1] == '*'){
                        dp[i][j] = prev[i];
                    }
                    else{
                        if(s[i - 1] == p[j - 1])dp[i][j] = dp[i - 1][j - 1];
                    }
                }

            }
            
            for(int i = 0;i <= n;i++){
                if(i == 0)prev[i] = dp[i][j];
                else prev[i] = prev[i - 1] | dp[i][j];
            }
        }
        return dp[n][m];
    }
};
```

#### [Allocate Mailboxes](https://leetcode.com/problems/allocate-mailboxes/)
+ Dynamic Prgramming: 
  - Time: O(N<sup>3</sup>)
  - Space: O(N<sup>2</sup>)
  
```c++
class Solution {
public:
    int minDistance(vector<int>& houses, int k) {
        sort(houses.begin(),houses.end());
        int n = houses.size();
        int dp[n + 1][k + 1];
        int medianCost[n][n];
        memset(medianCost,0,sizeof medianCost);
        
        for(int i = 0;i <= n;i++)for(int j = 0;j <= k;j++)dp[i][j] = 1e9;
        
        for(int i = 0;i < n;i++){
            for(int j = i;j < n;j++){
                int mid = (i + j)/2;
                for(int x = i;x <= j;x++){
                    medianCost[i][j] += abs(houses[mid] - houses[x]);
                }
            }
        }
        dp[n][0] = 0;
        for(int use = 1;use <= k;use++){
            for(int i = n - 1;i >= 0;i--){
                for(int j = i;j < n;j++){
                    dp[i][use] = min(dp[i][use],medianCost[i][j] + dp[j + 1][use - 1]);
                }
            }
        }
        return dp[0][k];
    }
};
```
