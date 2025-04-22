D K V V Varma
VU21CSEN0400060

1.[Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)



![[Pasted image 20241223150238.png]]


```C++
#include <queue>
#include <vector>
using namespace std;

class Solution {

public:

    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int num : nums) {
            pq.push(num);
            if (pq.size() > k) pq.pop();
        }
        return pq.top();

    }

};
```



2. [Design Circular Deque](https://leetcode.com/problems/design-circular-deque/)

![[Pasted image 20241223152235.png]]


```C++
class MyCircularDeque {

public:

    vector<int> dq;
    int front, rear, n, size;

    MyCircularDeque(int k) {
        n = k;
        dq.resize(k);
        front = 0;
        rear = 0;
        size = 0;
    }

    bool insertFront(int value) {
        if (size < n) {
            front = (front - 1 + n) % n;
            dq[front] = value;
            size++;
            return true;

        }
        return false;

    }

    bool insertLast(int value) {
        if (size < n) {
            dq[rear] = value;
            rear = (rear + 1) % n;
            size++;
            return true;
        }
        return false;

    }

    bool deleteFront() {
        if (size > 0) {
            front = (front + 1) % n;
            size--;
            return true;
        }
        return false;

    }

    bool deleteLast() {
        if (size > 0) {
            rear = (rear - 1 + n) % n;
            size--;
            return true;
        }
        return false;
    }
    int getFront() {
        if (size > 0) {
            return dq[front];
        }
        return -1;

    }

    int getRear() {
        if (size > 0) {
            return dq[(rear - 1 + n) % n];

        }
        return -1;
    }

    bool isEmpty() {
        return size == 0;
    }

    bool isFull() {
        return size == n;
    }

};
```


 3.[23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

![[Pasted image 20241223154020.png]]


```C++
class Solution {

public:

    ListNode* mergeKLists(vector<ListNode*>& lists) {

        if(lists.size() == 0) return NULL;

        ListNode* dummyHead = new ListNode(-1);
        ListNode* dummyTail = dummyHead;
        priority_queue<pair<int, ListNode*>, vector<pair<int, ListNode*>>, greater<pair<int, ListNode*>>> pq;
        for(auto head : lists)  if(head != NULL) pq.push({head->val, head});

        while(!pq.empty()){
            ListNode* minNode = pq.top().second;
            pq.pop();
            if(minNode->next != NULL) pq.push({minNode->next->val, minNode->next});

            dummyTail->next = minNode;
            dummyTail = dummyTail->next;

        }
        return dummyHead->next;

    }

};
```