

# pair

```cpp
pair<int, string> p = {3, "abc"};
```

構造化束縛。

```cpp
auto [number, text] = p;
```

# `vector<pair>`

```cpp
vector<pair<int, ll>> edges;

edges.push_back({2, 100});
edges.emplace_back(3, 200);
```

```cpp
for (const auto& [to, cost] : edges) {
    cout << to << ' ' << cost << '\n';
}
```

## pair の比較

`first` → `second` の順に辞書順比較される。

```cpp
sort(edges.begin(), edges.end());
```

# tuple

```cpp
tuple<int, string, ll> t = {1, "xyz", 100};
```

構造化束縛。

```cpp
auto [a, b, c] = t;
```

個別取得。

```cpp
int first = get<0>(t);
string second = get<1>(t);
```

## 関連
- [[STL/sort-search]]
- [[STL/queue-stack-deque-heap]]
