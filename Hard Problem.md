#### [Minimum Cost to Hire K Workers](https://leetcode.com/problems/minimum-cost-to-hire-k-workers/)
+ Priority Queue: 
  - Time: O(N  * log N)
  - Space: O(N)
```c++
class Solution {
public:
    double mincostToHireWorkers(vector<int>& quality, vector<int>& wage, int K) {
        vector<pair<double,double>>vec;
        for(int i = 0;i < wage.size();i++){
            vec.push_back(make_pair((double)wage[i]/quality[i],quality[i]));
        }
        sort(vec.begin(),vec.end());
        priority_queue<int>Q;
        double ans = 1e18, total_quality = 0;
        
        for(int i = 0;i < vec.size();i++){
            Q.push(vec[i].second);
            total_quality += vec[i].second;
            while(Q.size() > K){
                total_quality -= Q.top();
                Q.pop();
            }
            if(Q.size() == K){
                ans = min(ans,total_quality * vec[i].first);
            }
        }
        return ans;
    }
};
```



#### [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
+ Recursion: 
  - Time: O(N)
  - Space: O(N)
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* build(vector<int> &inorder, vector<int> &postorder,unordered_map<int,int>&mp,int l1,int r1,int l2,int r2){
        if(l1 > r1 || l2 > r2)return NULL;
        int value = postorder[r2];
        int pos = mp[value];
        
        TreeNode* root = new TreeNode(value);
        
        int ele = pos - l1;
        
        root->left = build(inorder,postorder,mp,l1,pos - 1,l2,l2 + ele - 1);
        root->right = build(inorder,postorder,mp,pos + 1,r1,l2 + ele,r2 - 1);
        return root;
    }
    TreeNode* buildTree(vector<int> &inorder, vector<int> &postorder) {
        unordered_map<int,int>mp;
        for(int i = 0;i < inorder.size();i++){
            mp[inorder[i]] = i;
        }
        TreeNode* root = build(inorder,postorder,mp,0,inorder.size() - 1,0,inorder.size() - 1);
        return root;
    }
};
```
