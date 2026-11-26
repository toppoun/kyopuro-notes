
# queue

FIFO。

```cpp
queue<int> q;

q.push(10);
q.push(20);

int x = q.front();
q.pop();
```

`pop()` は値を返さない。

# stack

LIFO。

```cpp
stack<int> st;

st.push(10);
st.push(20);

int x = st.top();
st.pop();
```

# deque

両端から追加・削除できる。

```cpp
deque<int> dq;

dq.push_back(10);
dq.push_front(20);

dq.pop_back();
dq.pop_front();
```

先頭・末尾。

```cpp
int a = dq.front();
int b = dq.back();
```

# priority_queue

デフォルトは最大ヒープ。

```cpp
priority_queue<int> pq;

pq.push(3);
pq.push(10);
pq.push(5);

int mx = pq.top();
pq.pop();
```

# min-heap

```cpp
priority_queue<int, vector<int>, greater<>> pq;
```

## pair の min-heap

ダイクストラなどでよく使う。

```cpp
priority_queue<
    pair<ll, int>,
    vector<pair<ll, int>>,
    greater<>
> pq;

pq.push({10, 2});
pq.push({3, 5});

auto [dist, v] = pq.top();
pq.pop();
```

`pair` は `first` → `second` の辞書順比較。

## 関連
- [[STL/pair-tuple]]
