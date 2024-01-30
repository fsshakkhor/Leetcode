
#### [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)
+ Heap: 
  - Time: O(N  * log N)
  - Space: O(N)
```c++
class FreqStack {
public:
    priority_queue<pair<int, pair<int, int>>> Q;
    unordered_map<int, int> freq;
    int pos = 0;
public:
    void push(int x) {
        Q.emplace(++freq[x], make_pair(++pos, x));
    }
    
    int pop() {
        auto val = Q.top();
        Q.pop();
        int x = val.second.second;
        freq[x]--;
        return x;
    }
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 */
```


#### [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)
+ Heap: 
  - Time: O(N  * log N)
  - Space: O(N)
```c++
class MedianFinder {
public:
    /** initialize your data structure here. */
    priority_queue<int>lft;
    priority_queue<int,vector<int>,greater<int>>rgt;
    MedianFinder() {
        
    }
    
    void addNum(int num) {
        if(!rgt.empty() and num > rgt.top()){
            rgt.push(num);
        }else{
            lft.push(num);
        }
        if(lft.size() - rgt.size() == 2){
            rgt.push(lft.top());
            lft.pop();
        }else if(rgt.size() - lft.size() == 2){
            lft.push(rgt.top());
            rgt.pop();
        }

    }
    
    double findMedian() {
        if(lft.size() == rgt.size()){
            double sum = lft.top() + rgt.top();
            return sum/2;
        }
        if(lft.size() > rgt.size())return lft.top();
        return rgt.top();

    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```


#### [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)
+ Heap: 
  - Time: Average O(1)
  - Space: O(N)
```c++
class RandomizedSet {
public:
    unordered_map<int,int>loc;
    vector<int>Array;
    
    /** Initialize your data structure here. */
    RandomizedSet() {
        
    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        if(loc.find(val) != loc.end())return false;
        loc[val] = Array.size();
        Array.push_back(val);
        return true;
    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val) {
        if(loc.find(val) == loc.end())return false;
        int position = loc[val];
        int last = Array.size() - 1;
        
        swap(Array[last],Array[position]);
        loc[Array[position]] = position;
        
        loc.erase(Array[last]);
        Array.pop_back();
        return true;
    }
    
    /** Get a random element from the set. */
    int getRandom() {
        return Array[rand() % Array.size()];
    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```
