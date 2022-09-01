# Design

#### [2034. Stock Price Fluctuation](https://leetcode.cn/problems/stock-price-fluctuation/)

timestamp, stock, $n$ updates

(1) update in the map $O(1)$

(2) maxPrice: pop until `mp[timestamp]==price` , amortized time complexity $O(\log (n))$ 

space: $O(n)$

```python
typedef pair<int, int> pii;

class StockPrice {
public:
    StockPrice() {
        currTime = 0;
    }
    
    void update(int timestamp, int price) {
        currTime = max(currTime, timestamp);
        mp[timestamp] = price;
        qMax.push(pii(price, timestamp));
        qMin.push(pii(price, timestamp));
    }
    
    int current() {
        return mp[currTime];
    }
    
    int maximum() {
        while (qMax.top().first != mp[qMax.top().second]){
            qMax.pop();
        }
        return qMax.top().first;
    }
    
    int minimum() {
        while (qMin.top().first != mp[qMin.top().second]){
            qMin.pop();
        }
        return qMin.top().first;
    }
private:
    int currTime;
    map<int, int> mp; // from time to price
    priority_queue<pii, vector<pii>, less<pii>> qMax;
    priority_queue<pii, vector<pii>, greater<pii>> qMin;
};
```


