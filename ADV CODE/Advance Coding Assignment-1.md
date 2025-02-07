D K V V Varma
VU21CSEN0400060

[114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

![[Pasted image 20241219220058.png]]

```C++
class Solution {

private:

    TreeNode* prev = nullptr;

public:

    void flatten(TreeNode* root) {

        if (!root) return;

        flatten(root->right);

        flatten(root->left);

        root->right = prev;

        root->left = nullptr;

        prev = root;

    }

};
```


2.[42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

![[Pasted image 20241219220435.png]]

```C++
class Solution {

public:

    int trap(vector<int>& height) {

        int left = 0, right = height.size() - 1;
        int leftMax = INT_MIN, rightMax = INT_MIN;
        int totalWater = 0;

        while (left < right) {
            leftMax = max(leftMax, height[left]);
            rightMax = max(rightMax, height[right]);
            if (leftMax < rightMax) {
                totalWater += leftMax - height[left];
                left++;
                
            } else {
                totalWater += rightMax - height[right];
                right--;
            }
        }
        return totalWater;

    }

};
```

