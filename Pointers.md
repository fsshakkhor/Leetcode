#### [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
+ Divide and Conquer: 
  - Time: O(N * log N)
  - Space: O(N)
  
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* merge(ListNode* list1,ListNode* list2){
        if(list1 == NULL and list2 == NULL)return NULL;
        if(list2 == NULL){
            ListNode* head = new ListNode(list1->val);
            head->next = merge(list1->next,list2);
            return head;
        }
        if(list1 == NULL){
            ListNode* head = new ListNode(list2->val);
            head->next = merge(list1,list2->next);
            return head;
        }
        if(list1->val > list2->val)swap(list1,list2);
        
        ListNode* head = new ListNode(list1->val);
        head->next = merge(list1->next,list2);
        return head;
        
    }
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if(lists.empty())return NULL;
        
        while(lists.size() > 1){
            vector<ListNode*>temp;
            for(int i = 0;i < lists.size();i += 2){
                if(i + 1 < lists.size())temp.push_back(merge(lists[i],lists[i + 1]));
                else temp.push_back(lists[i]);
            }
            lists = temp;
        }
        
        return lists[0];
    }
};
```



#### [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)
+ HashTable: 
  - Time: O(N)
  - Space: O(N)
  
```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    unordered_map<Node*,Node*>mp;
    Node* copyRandomList(Node* head) {
        if(head == NULL)return NULL;
        if(mp.find(head) != mp.end())return mp[head];
        
        Node* temp = new Node(head->val);
        mp[head] = temp;
        temp->next = copyRandomList(head->next);
        temp->random = copyRandomList(head->random);
        
        return mp[head];
    }
};
```

#### [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
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
    vector<vector<int>>vec;
    void dfs(TreeNode* node,int lev){
        if(node == NULL)return;
        
        if(vec.size() <= lev)vec.push_back({node->val});
        else{
            vec[lev].push_back(node->val);
        }
        dfs(node->left,lev + 1);
        dfs(node->right,lev + 1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        dfs(root,0);
        return vec;
    }
};
```



#### [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
+ Recursion: 
  - Time: O(N)
  - Space: O(1)
  
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int getLevel(TreeNode* root,TreeNode *me,TreeNode *she,TreeNode* &ans){
        if(root == NULL)return 0;
        int ret = 0;
        if(root->val == me->val)ret += 1;
        if(root->val == she->val)ret += 1;
        ret = ret + getLevel(root->left,me,she,ans);
        ret = ret + getLevel(root->right,me,she,ans);
        if(ret == 2 and ans == NULL){
            ans = root;
        }
        return ret;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* ans = NULL;
        int g = getLevel(root,p,q,ans);
        return ans;
    }
};
```

#### [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
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
    vector<int>g[105];
    void dfs(TreeNode* root,int lev){
        if(root == NULL)return;
        g[lev].push_back(root->val);
        dfs(root->left,lev + 1);
        dfs(root->right,lev + 1);
    }
    vector<int> rightSideView(TreeNode* root) {
        vector<int>ans;
        dfs(root,0);
        for(int i = 0;i < 100;i++){
            if(g[i].empty())break;
            ans.push_back(g[i].back());
        }
        return ans;
    }
};
```


#### [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
+ DFS: 
  - Time: O(N)
  - Space: O(N)
  
```c++
/**
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
    int dfs(TreeNode* root,int &ans){
        if(root == NULL)return 0;
        int lft = dfs(root->left,ans);
        int rgt = dfs(root->right,ans);
        int ret = root->val + max(0,lft) + max(0,rgt);
        ans = max(ans,ret);
        return root->val + max(0,max(lft,rgt));
    }
    int maxPathSum(TreeNode* root) {
        int ans = -1e9;
        dfs(root,ans);
        return ans;
        
    }
};
```
