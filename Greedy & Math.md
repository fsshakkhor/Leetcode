#### [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)
+ Greedy: 
  - Time: O(N + M)
  - Space: O(1)
  
```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size();
        int m = matrix[0].size();

        int x = 0, y = m - 1;
        while(x < n and y >= 0){
            if(matrix[x][y] == target)return true;
            if(matrix[x][y] > target)y--;
            else x++;
        }
        return false;
    }
};
```

#### [Jump Game](https://leetcode.com/problems/jump-game/)
+ Greedy: 
  - Time: O(N)
  - Space: O(1)
  
```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int reach = n - 1;
        for(int i = n - 2;i >= 0;i--){
            if(i + nums[i] >= reach){
                reach = i;
            }
        }
        return reach == 0;
    }
};
```

#### [Merge Intervals](https://leetcode.com/problems/merge-intervals)
+ Greedy: 
  - Time: O(N * log N)
  - Space: O(N)
  
```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end());
        vector<vector<int>>ans;
        ans.push_back(intervals[0]);
        for(int i = 1;i < intervals.size();i++){
            if(intervals[i][0] <= ans.back()[1]){
                int l = min(intervals[i][0],ans.back()[0]);
                int r = max(intervals[i][1],ans.back()[1]);
                ans.pop_back();
                ans.push_back({l,r});
            }else{
                ans.push_back(intervals[i]);
            }
        }
        return ans;
    }
};

```

#### [Minimum Time Visiting All Points](https://leetcode.com/problems/minimum-time-visiting-all-points/)
+ Greedy: 
  - Time: O(N)
  - Space: O(1)
  
```c++
class Solution {
public:
    int minTimeToVisitAllPoints(vector<vector<int>>& points) {
        int ans = 0;
        for(int i = 1;i < points.size();i++){
            int dx = points[i][0] - points[i - 1][0];
            int dy = points[i][1] - points[i - 1][1];
            ans += max(abs(dx),abs(dy));
        }
        return ans;
    }
};

```


#### [Bulls and Cows](https://leetcode.com/problems/bulls-and-cows/)
+ Brute Force: 
  - Time: O(string_length<sup>2</sup>)
  - Space: O(1)
  
```c++
class Solution {
public:
    string getHint(string secret, string guess) {
        int bulls = 0;
        for(int i = 0;i < secret.size();i++){
            if(secret[i] == guess[i])bulls++;
        }
        int cows = 0;
        for(int i = 0;i < secret.size();i++){
            if(secret[i] == guess[i])continue;
            for(int j = 0;j < guess.size();j++){
                if(secret[j] == guess[j])continue;
                if(guess[i] == secret[j]){
                    guess[i] = secret[j] = '-';
                    cows++;
                    break;
                }
            }
        }
        string s = to_string(bulls) + 'A' + to_string(cows) + 'B';
        return s;
        
    }
};
```


#### [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)
+ Greedy + Sorting: 
  - Time: O(N * log N)
  - Space: O(1)
  
```c++
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        sort(points.begin(),points.end(),[](vector<int>a,vector<int>b){
            int x = a[0] * a[0] + a[1] * a[1];
            int y = b[0] * b[0] + b[1] * b[1];
            return x < y;
        });
        points.resize(K);
        return points;
    }
};
```
