D K V V Varma
VU21CSEN0400060

1.[936. Stamping The Sequence](https://leetcode.com/problems/stamping-the-sequence/)


![[Pasted image 20241230151148.png]]


```C++
```cpp
class Solution {
    int n, m;
private:
    int findInd(string stamp, string target){
        for(int i=0; i<=m-n; i++){
            int s=0, t=i;
            bool flag = false;
            while(s<n && t<m && (target[t]=='?' || target[t]==stamp[s])){
                if(target[t]!='?') flag=true;
                s++, t++;
            }
            if(s==n && flag) return i;
        }
        return -1;
    }
public:
    vector<int> movesToStamp(string stamp, string target) {
        n = stamp.size(), m = target.size();
        string temp = "";
        for(int i=0; i<m; i++) temp+='?';
        vector<int> ans;

        while(temp!=target){
            int ind = findInd(stamp,target);
            if(ind<0) return {};
            else{
                ans.push_back(ind);
                for(int i=ind; i<ind+n; i++)
                    target[i] = '?';
            }
        }

        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```



2.[918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)


![[Pasted image 20241230152518.png]]

```C++
class Solution {

public:

    int maxSubarraySumCircular(vector<int>& A) {
        int total_sum=0,curr_sum1=0,curr_sum2=0,mxsum_subary=INT_MIN,minsum_subary=INT_MAX;  

        for(auto i:A)

        {

            total_sum+=i; curr_sum1+=i; curr_sum2+=i;
            mxsum_subary=max(mxsum_subary,curr_sum1);
            if(curr_sum1<0) curr_sum1=0;
           minsum_subary=min(curr_sum2,minsum_subary);
            if(curr_sum2>0) curr_sum2=0;

        }

        return (total_sum==minsum_subary)?mxsum_subary:max(mxsum_subary,total_sum-minsum_subary);  

    }

};
```