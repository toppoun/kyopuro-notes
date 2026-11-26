
## 基本

```cpp
string s = "abcdef";

char c = s[2];
string sub = s.substr(1, 3); // index 1 から長さ3 → "bcd"

s.push_back('g');
s.pop_back();

int n = (int)s.size();
bool empty = s.empty();
```

## find

左から文字列・文字を探す。見つからない場合は `string::npos`。

```cpp
auto pos = s.find("cd");

if (pos != string::npos) {
    cout << pos << '\n';
}
```

開始位置も指定できる。

```cpp
auto pos = s.find("cd", 3);
```

## rfind

右側にある最後の出現位置を探す。

```cpp
auto pos = s.rfind("abc");
```

## find_first_of / find_last_of

指定した文字の「どれか1文字」を探す。

```cpp
auto p1 = s.find_first_of("abc");
auto p2 = s.find_last_of("abc");
```

`find("abc")` は `"abc"` という連続部分文字列を探す。

`find_first_of("abc")` は `'a'`, `'b'`, `'c'` のどれかを探す。

## find_first_not_of / find_last_not_of

指定した文字以外を探す。

```cpp
string s = "000123000";

auto first = s.find_first_not_of('0');
auto last = s.find_last_not_of('0');
```

## starts_with / ends_with

C++20以降。

```cpp
bool pre = t.starts_with(s);
bool suf = t.ends_with(s);
```

## 大文字・小文字

`toupper` / `tolower` は1文字に対して使う。

```cpp
for (char& c : s) {
    c = toupper(c);
}
```

```cpp
for (char& c : s) {
    c = tolower(c);
}
```

文字列そのものを `toupper(s)` とはできない。

大文字かどうかの判定は `isupper()`、小文字かどうかの判定は `islower()` を使う。

```cpp
if (isupper(c)) {
    // c は大文字
}
```

```cpp
if (islower(c)) {
    // c は小文字
}
```

英字だけを扱うなら、範囲で直接判定してもよい。

```cpp
if ('A' <= c && c <= 'Z') {
    // 大文字
}
```

```cpp
if ('a' <= c && c <= 'z') {
    // 小文字
}
```
## reverse

```cpp
reverse(s.begin(), s.end());
```

回文判定の簡単な書き方。

```cpp
string t = s;
reverse(t.begin(), t.end());

bool palindrome = (s == t);
```

## 数字文字との変換

```cpp
int x = c - '0';
char c = '0' + x;
```

## 関連
- [[STL/sort-search]]
- [[Manacher法（回文）]]
