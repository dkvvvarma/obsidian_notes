
D K V V Varma
VU21CSEN0400060



1. [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

![[Pasted image 20250324150036.png]]

```c++
class Solution {
public:
int climbStairs(int n, unordered_map<int, int>& memo) {
if (n == 0 || n == 1) {
return 1;
}

if (memo.find(n) == memo.end()) {
memo[n] = climbStairs(n-1, memo) + climbStairs(n-2, memo);
}

return memo[n];

}

int climbStairs(int n) {
unordered_map<int, int> memo;
return climbStairs(n, memo);

}

};

```


![[Pasted image 20250324145918.png]]


2. [329. Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

![[Pasted image 20250324150147.png]]


```C++
class Solution {
public:
    int dfs(int row, int col, vector<vector<int>>& matrix, vector<vector<int>>& dp){
        int m = matrix.size(), n = matrix[0].size();
        int ans = 1;
        if(dp[row][col] != -1) return dp[row][col];
        for(int tempR=-1; tempR<=1; tempR++){
            for(int tempC=-1; tempC<=1; tempC++){
                if(abs(tempR) + abs(tempC) != 1) continue;
                int Nrow = row + tempR;
                int Ncol = col + tempC;
                if(Nrow>=0 && Nrow<m && Ncol>=0 && Ncol<n && matrix[Nrow][Ncol] > matrix[row][col]){
                    ans = max(ans, 1+dfs(Nrow, Ncol, matrix, dp));
                }
            }
        }
        return dp[row][col] = ans;
    }
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        int result = 0;
        vector<vector<int>> dp(m, vector<int>(n, -1));
        for(int i=0; i<m; i++){
            for(int j=0; j<n; j++){
                result = max(result, dfs(i, j, matrix, dp));
            }
        }
        return result;
    }
};
```




![[Pasted image 20250324150222.png]]