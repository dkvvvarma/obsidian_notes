D K V V Varma
VU21CSEN0400060

1.[1472. Design Browser History](https://leetcode.com/problems/design-browser-history/)

![[Pasted image 20250106142220.png]]


```C++
#define pb push_back
class BrowserHistory {
public:
    int ele;
    vector<string> vc;
    BrowserHistory(string homepage) {
        vc.pb(homepage);
        ele  =0;
    }
    void visit(string url) {
        int l = vc.size()-1;
        while(l>ele){
            vc.pop_back();
            l--;
        }
        ele++;
        vc.pb(url);
    }
    string back(int steps) {
        ele-=steps;
        if(ele<0) ele=0;
        return vc[ele];
    }
    string forward(int steps) {
        ele +=steps;
        if(ele>=vc.size()) ele = vc.size()-1;
        return vc[ele];
    }
};
```



2.[146. LRU Cache](https://leetcode.com/problems/lru-cache/)

![[Pasted image 20250106143228.png]]

```C++
#include <unordered_map>
#include <list>
class LRUCache {

private:
    int capacity;
    std::list<std::pair<int, int>> cache;
    std::unordered_map<int, std::list<std::pair<int, int>>::iterator> cacheMap;
    
public:
    LRUCache(int capacity) {
        this->capacity = capacity;
    }
    int get(int key) {
        if (cacheMap.find(key) == cacheMap.end()) {
            return -1;
        }
        auto it = cacheMap[key];
        cache.splice(cache.begin(), cache, it);
        return it->second;
    }
    void put(int key, int value) {
        if (cacheMap.find(key) != cacheMap.end()) {
            auto it = cacheMap[key];
            it->second = value;
            cache.splice(cache.begin(), cache, it);
            return;
        }
        if (cache.size() == capacity) {
            auto lru = cache.back();
            cacheMap.erase(lru.first);
            cache.pop_back();
        }
        cache.emplace_front(key, value);
        cacheMap[key] = cache.begin();
    }
};
```

