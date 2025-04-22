D K V V Varma
VU21CSEN0400060

### Hashing

1.  [1461. Check If a String Contains All Binary Codes of Size K](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)

![[Pasted image 20250120144250.png]]


```C++
class Solution {
public:
    bool hasAllCodes(string s, int k) {
        unordered_set<string> res;
        int n = pow(2,k);
        if(k>s.size()) return false;
        for(int i=0;i<=s.size()-k;i++){
            res.insert(s.substr(i,k));
            if(res.size()==n) return true;
        }
        return false;
    }
};
```



2.[1147. Longest Chunked Palindrome Decomposition](https://leetcode.com/problems/longest-chunked-palindrome-decomposition/)

![[Pasted image 20250120144509.png]]


```C++
class Solution {
public:
    int longestDecomposition(string text) {
        return helper(text, 0, text.length() - 1);
    }
    int helper(string& text, int left, int right) {
        if (left > right) {
            return 0;
        }
        for (int i = 1; i <= (right - left + 1) / 2; ++i) {
            if (text.substr(left, i) == text.substr(right - i + 1, i)) {
                return 2 + helper(text, left + i, right - i);
            }
        }
        return 1;
    }
};
```



3. [1425. Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/)

![[Pasted image 20250120144841.png]]


```C++
class Solution {
public:
    int constrainedSubsetSum(vector<int>& nums, int k) {
        int n = nums.size();
        int maxSum = nums[0];
        deque<pair<int,int>> dp;
        dp.push_back({0, nums[0]});
        for(int i = 1; i < n; i++)
        {
            if(i - dp.front().first > k)
                dp.pop_front();
            int current = nums[i];
            if(dp.front().second > 0)
                current += dp.front().second;
            maxSum = max(current, maxSum);
            while(!dp.empty() && dp.back().second < current)
                dp.pop_back();
            dp.push_back({i, current});
        }
        return maxSum;
    }
};
```


4.[1499. Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

![[Pasted image 20250120145753.png]]


```C++
class Solution {
public:
    int findMaxValueOfEquation(vector<vector<int>>& nums, int k) {
       int max=INT_MIN;
        int i=0,j=1;
        while(i < j && j<nums.size()){
            long long int diff =(long long) nums[j][0] - (long long)nums[i][0];
            if(diff <= k){
                long long int eq = (long long) nums[j][1] + (long long)nums[i][1] + diff;
                if( max < eq) max = eq;
                if(nums[j][1] - nums[i][1] > nums[j][0] - nums[i][0]){
                    i = j;
                }
                j++;
            }else{
                if(j = i+1){
                    j++;
                }
                i++;
            }
        }
       return max;
    }
};
```