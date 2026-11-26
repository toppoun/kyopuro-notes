
整数をbit集合として扱うときによく使う。

```cpp
ll x = 0;
```

## i bit目を見る

```cpp
bool on = (x >> i) & 1LL;
```

## i bit目を1にする

```cpp
x |= 1LL << i;
```

## i bit目を0にする

```cpp
x &= ~(1LL << i);
```

## i bit目を反転する

```cpp
x ^= 1LL << i;
```

## AND / OR / XOR

```cpp
a & b
a | b
a ^ b
```

## 1の個数

```cpp
int cnt = __builtin_popcountll(x);
```

## 最下位の1bit

```cpp
ll low = x & -x;
```

## 最下位の1bitを消す

```cpp
x &= x - 1;
```

関連: [[アルゴリズム/bit全探索]]
