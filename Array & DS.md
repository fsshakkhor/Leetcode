#### [3 Sum](https://leetcode.com/problems/3sum/)
+ Two Pointers: 
  - Time: O(N<sup>2</sup>)
  - Space: O(N)
```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        int n = nums.size();

        vector<vector<int>>ans;

        for(int i = 0;i + 2 < n;i++){
            if(i > 0 and nums[i] == nums[i - 1])continue;
            int ptr = n - 1;
            for(int j = i + 1;j + 1 < n;j++){
                while(ptr > 0 and nums[i] + nums[j] + nums[ptr] > 0)ptr--;
                if(j >= ptr)break;
                if(nums[i] + nums[j] + nums[ptr] == 0){
                    vector<int>p = {nums[i],nums[j],nums[ptr]};
                    if(!ans.empty() and ans.back() == p)continue;
                    ans.push_back(p);
                }
            }
        }
        return ans;
    }
};
```
#### [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
+ Binary Search: 
  - Time: O(log N)
  - Space: O(1)
  
```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int lo = 0 , hi = nums.size() - 1;
        while (lo <= hi){
            int mid = (lo + hi) >> 1;
            
            if(nums[mid] == target)return mid;
            
            if(nums[lo] <= nums[mid]){
                if(target > nums[mid] or target < nums[lo]){
                    lo = mid + 1;
                }else{
                    hi = mid - 1;
                }
            }else{
                if(target > nums[hi] or target < nums[mid + 1]){
                    hi = mid - 1;
                }else{
                    lo = mid + 1;
                }
            }
        }
        return -1;
    }
};
```


#### [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
+ Binary Search: 
  - Time: O(log N)
  - Space: O(1)
  
```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int lo = 0 , hi = nums.size() - 1;
        
        int L = -1,R = -1;
        while(lo <= hi){
            int mid = (lo + hi) >> 1;
            if(nums[mid] < target){
                lo = mid  + 1;
            }else if(nums[mid] > target){
                hi = mid - 1;
            }else{
                L = mid;
                hi = mid - 1;
            }
        }
        lo = 0 , hi = nums.size() - 1;
        while(lo <= hi){
            int mid = (lo + hi) >> 1;
            if(nums[mid] < target){
                lo = mid  + 1;
            }else if(nums[mid] > target){
                hi = mid - 1;
            }else{
                R = mid;
                lo = mid + 1;
            }
        }
        return {L,R};
        

    }
};
```







#### [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
+ Priority Queue/Heap: 
  - Time: O(N * log K)
  - Space: O(K)
+ Quick Sort:
  - Time: O(N)
  - Space: O(1)

```c++
class Solution {
public:
    
    int fix(int l,int r,vector<int>&nums){
        int value = nums[r];
        int ptr = l;
        for(int i = l;i < r;i++){
            if(nums[i] <= value){
                swap(nums[i],nums[ptr]);
                ptr++;
            }
        }
        swap(nums[ptr],nums[r]);
        return ptr;
        
    }
    void solve(int l,int r,vector<int>&nums,int k){
        if(l >= r)return;
        if(k >= l and k <= r){
            int pivot = fix(l,r,nums);
            solve(l,pivot - 1,nums,k);
            solve(pivot + 1,r,nums,k);
        }else{
            return;
        }
    }
    int findKthLargest(vector<int>& nums, int k) {
        random_shuffle(nums.begin(),nums.end());
        k = nums.size() - k;
        solve(0,nums.size() - 1,nums,k);

        return nums[k];
    }
};
```

#### [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
+ Quick Sort:
  - Time: O(N<sup>2</sup>)
  - Space: O(1)

```c++
class Solution {
public:
    int fix(int l,int r,vector<int>&nums){
        int value = nums[r];
        int ptr = l;
        for(int i = l;i < r;i++){
            if(nums[i] <= value){
                swap(nums[i],nums[ptr]);
                ptr++;
            }
        }
        swap(nums[ptr],nums[r]);
        return ptr;
        
    }
    void solve(int l,int r,vector<int>&nums,int k){
        if(l >= r)return;
        if(k >= l and k <= r){
            int pivot = fix(l,r,nums);
            solve(l,pivot - 1,nums,k);
            solve(pivot + 1,r,nums,k);
        }else{
            return;
        }
    }
    
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        vector<int>nums;
        for(int i = 0;i < matrix.size();i++){
            for(int j = 0;j < matrix.size();j++){
                nums.push_back(matrix[i][j]);
            }
        }
        k--;
        random_shuffle(nums.begin(),nums.end());
        solve(0,nums.size() - 1,nums,k);
        return nums[k];
    }
};


```




#### [Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)
+ Stack:
  - Time: O(N)
  - Space: O(N)

```c++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        stack<int>L,R;
        const int N = 100005;
        int lft[N],rgt[N];
        memset(lft,-1,sizeof lft);
        memset(rgt,-1,sizeof rgt);

        for(int i = 0;i < nums.size();i++){
            while(!L.empty() and nums[L.top()] >= nums[i]){
                L.pop();
            }
            if(!L.empty())lft[i] = L.top();
            L.push(i);
        }
        for(int i = nums.size() - 1;i >= 0;i--){
            while(!R.empty() and nums[i] >= nums[R.top()]){
                R.pop();
            }
            if(!R.empty())rgt[i] = R.top();
            R.push(i);
        }

        for(int i = 0;i < nums.size();i++){
            if(lft[i] != -1 and rgt[i] != -1)return true;
        }
        return false;
    }
};


```


#### [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
+ HashMap:
  - Time: O(N)
  - Space: O(N)

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int>st;
        for(int i : nums)st.insert(i);
        
        int ans = 0;
        for(int val : st){
            if(st.find(val - 1) == st.end()){
                int streak = 1;
                int cur = val;
                while(st.find(cur + 1) != st.end()){
                    streak++;
                    cur++;
                }
                ans = max(ans,streak);
            }
        }
        return ans;
    }
};
```


#### [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
+ Stack:
  - Time: O(N)
  - Space: O(N)

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        heights.push_back(0);
        stack<int>st;
        int ans = 0;
        for(int i = 0;i < heights.size();i++){

            while(!st.empty() and heights[st.top()] >= heights[i]){
                int h = heights[st.top()];
                st.pop();
                
                int x = -1;
                if(!st.empty())x = st.top();
                ans = max(ans,(i - x - 1) * h);
            }
            st.push(i);
        }
        return ans;
    }
};
```

#### [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
+ Stack:
  - Time: O(N)
  - Space: O(N)

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        vector<int>pref(n),suf(n);
        for(int i = 0;i < n;i++){
            if(i > 0){
                pref[i] = max(pref[i - 1],height[i]);
            }else{
                pref[i] = height[i];
            }
        }
        for(int i = n - 1;i >= 0;i--){
            if(i == n - 1){
                suf[i] = height[i];
            }else{
                suf[i] = max(suf[i + 1],height[i]);
            }
        }
        int ans = 0;
        for(int i = 1;i < n - 1;i++){
            int base = min(pref[i - 1],suf[i + 1]);
            if(height[i] < base){
                ans = ans + base - height[i];
            }
        }
        return ans;
    }
};
```


#### [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)
+ Stack:
  - Time: O(N<sup>2</sup>)
  - Space: O(N<sup>2</sup>)

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        heights.push_back(0);
        stack<int>st;
        int ans = 0;
        for(int i = 0;i < heights.size();i++){

            while(!st.empty() and heights[st.top()] >= heights[i]){
                int h = heights[st.top()];
                st.pop();
                
                int x = -1;
                if(!st.empty())x = st.top();
                ans = max(ans,(i - x - 1) * h);
            }
            st.push(i);
        }
        return ans;
    }
    int maximalRectangle(vector<vector<char>>& matrix) {
        int n = matrix.size();
        if(n == 0)return 0;
        int m = matrix[0].size();
        
        int dp[n][m];
        memset(dp,0,sizeof dp);
        int ans = 0;
        for(int i = 0;i < n;i++){
            vector<int>heights;
            for(int j = 0;j < m;j++){
                if(i == 0)dp[i][j] = matrix[i][j] == '1';
                else{
                    if(matrix[i][j] == '1')dp[i][j] = dp[i - 1][j] + 1;
                    else dp[i][j] = 0;
                }
                heights.push_back(dp[i][j]);
            }
            ans = max(ans,largestRectangleArea(heights));
        }
        return ans;
    }
};
```


#### [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)
+ Heap:
  - Time: O(N * log N)
  - Space: O(N)

```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        priority_queue<long long,vector<long long>,greater<long long>>Q;
        unordered_map<int,int>mp;
        int counter = 0;
        
        Q.push(1);
        while(counter < n){
            long long val = Q.top();
            Q.pop();
            if(mp[val])continue;
            mp[val] = 1;
            counter++;
            if(counter == n)return val;
            Q.push(val * 2);
            Q.push(val * 3);
            Q.push(val * 5);
        }
        return 0;
    }
};
```

#### [Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/)
+ Deque Technique:
  - Time: O(N)
  - Space: O(N)

```c++
class Solution {
public:
    int constrainedSubsetSum(vector<int>& nums, int k) {
        deque<int>Q;
        
        int n = nums.size();
        int dp[n];
        memset(dp,0,sizeof dp);
        int ans = -1e9;
        for(int i = 0;i < n;i++){
            int ptr = i - k;
            

            
            int add = 0;
            if(!Q.empty())add = max(add,dp[Q.front()]);

            dp[i] = nums[i] + add;

            if(!Q.empty() and Q.front() <= ptr){
                Q.pop_front();
            }
            
            while(!Q.empty() and dp[Q.front()] <= dp[i]){
                Q.pop_front();
            }
            while(!Q.empty() and dp[Q.back()] <= dp[i]){
                Q.pop_back();
            }
            Q.push_back(i);
            
            ans = max(ans,dp[i]);
        }
        return ans;
    }
};
```
