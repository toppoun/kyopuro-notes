
# set

常にソートされた重複なし集合。

```cpp
set<int> st;

st.insert(3);
st.insert(1);
st.insert(3);
```

存在判定。

```cpp
bool ok = st.contains(3);
```

イテレータが必要なら `find`。

```cpp
auto it = st.find(1);

if (it != st.end()) {
    st.erase(it);
}
```

`x` 以上の最小要素。

```cpp
auto it = st.lower_bound(x);

if (it != st.end()) {
    cout << *it << '\n';
}
```

`set` 自身の `lower_bound()` を使う。

# map

キー順に管理される連想配列。

```cpp
map<string, int> mp;

mp["apple"]++;
mp["banana"] = 5;
```

存在判定。

```cpp
if (mp.contains("apple")) {
}
```

`mp[key]` は存在しないキーでも作成してしまう。

値を作りたくない場合は `contains` / `find` を使う。

```cpp
auto it = mp.find("banana");

if (it != mp.end()) {
    cout << it->first << ' ' << it->second << '\n';
}
```

range-for。

```cpp
for (const auto& [key, value] : mp) {
    cout << key << ' ' << value << '\n';
}
```

# unordered_map

平均 O(1)。順序は保証されない。

```cpp
unordered_map<int, int> freq;

freq[10]++;
freq[20] += 3;
```

## 関連
- [[STL/sort-search]]
- [[STL/pair-tuple]]
