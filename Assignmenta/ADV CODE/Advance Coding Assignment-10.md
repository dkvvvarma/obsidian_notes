
D K V V Varma
VU21CSEN0400060

1. [638. Shopping Offers](https://leetcode.com/problems/shopping-offers/)

![[Pasted image 20250324150941.png]]

```C++
class Solution {
    unordered_map<string, int> dp;
    string getkey(vector<int>& needs, int index){
        string s;
        s += to_string(index) + ',';
        for(int i = 0;i<needs.size();i++){
            s += to_string(needs[i]) + ',';
        }
        return s;
    }
    int solve(vector<int>& price, vector<vector<int>>& special, vector<int>& needs, int index){
        string key = getkey(needs, index);
        if(dp.find(key) != dp.end()) return dp[key];
        if(index == special.size()){
            int ans = 0;
            for(int i = 0;i<price.size();i++){
                ans += price[i] * needs[i];
            }
            return ans;
        }
        int n = special[index].size();
        vector<int> newneed = needs;
        bool check = true;
        int sum = 0;
        for(int i = 0;i<n-1;i++){
            if(special[index][i] > needs[i]){
                check = false;
                break;
            }
            newneed[i] -= special[index][i];
            sum += special[index][i] * price[i];
        }
        int a = INT_MAX, b = INT_MAX, c = INT_MAX;
        if(sum > special[index][n-1] && check == true){
            a = special[index][n-1] + solve(price, special, newneed, index);
            b = special[index][n-1] + solve(price, special, newneed, index + 1);
        }
        c = solve(price, special, needs, index + 1);
        return dp[key] = min({a,b,c});
    }
public:
    int shoppingOffers(vector<int>& price, vector<vector<int>>& special, vector<int>& needs) {
        return solve(price, special, needs, 0);
    }
};
```

![[Pasted image 20250324151117.png]]


2.  [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/)

![[Pasted image 20250324151236.png]]

```C++
class Solution {
public:
    TreeNode* first = nullptr;  // First misplaced node
    TreeNode* second = nullptr; // Second misplaced node
    TreeNode* prev = nullptr;   // Tracks previous node in inorder 

    void inorder(TreeNode* root) {
        if (!root) return;

        inorder(root->left);

        if (prev && prev->val > root->val) {
            if (!first) first = prev; 
            second = root;            
        }
        prev = root; 

        inorder(root->right);
    }

    void recoverTree(TreeNode* root) {
        inorder(root);

        if (first && second) {
            swap(first->val, second->val);
        }
    }
};
```

![[Pasted image 20250324151424.png]]


4. [95. Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

![[Pasted image 20250324151457.png]]

```C++
class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        if (n == 0) return {};
        return generateTrees(1, n);
    }
    
    vector<TreeNode*> generateTrees(int start, int end) {
        vector<TreeNode*> trees;
        if (start > end) {
            trees.push_back(NULL);
            return trees;
        }
        
        for (int i = start; i <= end; ++i) {
            vector<TreeNode*> leftSubtrees = generateTrees(start, i - 1);
            vector<TreeNode*> rightSubtrees = generateTrees(i + 1, end);
            
            for (TreeNode* left : leftSubtrees) {
                for (TreeNode* right : rightSubtrees) {
                    TreeNode* root = new TreeNode(i);
                    root->left = left;
                    root->right = right;
                    trees.push_back(root);
                }
            }
        }
        
        return trees;
    }
};
```

![[Pasted image 20250324151609.png]]


