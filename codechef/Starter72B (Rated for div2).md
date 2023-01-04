# Starter72B (Rated for div2)

这场打得很窘迫。

## A

1可以作为除数，接着我们考虑它的两个不同因数是否存在即可

```cpp
void solve() {
    int n;
    cin >> n;
    for (int i = 2; i <= sqrt(n); i++) {
        if (n % i==0 && n/i != i){
            cout << i << " " << n/i << " " << 1 << "\n";
            return;
        }
    }
    cout << -1 << "\n";
}
```



## B

公式：∑(Ai * K^{i-1}) = S

没有想到证明，只是一开始想用背包做，但是`1≤S≤1e18`实在太大了，于是转而思考别的做法，最后尝试了贪心的做法，

先将S减至为≤0，接着从大到小依次减去相应的数字，看看是否能使其为0.

```cpp
void solve() {
    ll n, k, s;
    cin >> n >> k >> s;
    int ed = 0;
    ll base = 1;
    vector <int> ans(n);
    for (int i = 0; i < n; i++) {
        s -= base;
        ed = i;
        if (s <= 0) break;
        base *= k;
    }
    s = -s;
    fill(begin(ans), begin(ans)+ed+1,1);
    for (int i = ed; i >= 0; i--) {
        if (s >= base) {
            if (s >= 2 * base) ans[i] = -1, s -= 2 * base;
            else ans[i] = 0, s -= base;
        }
        base /= k;
    }
    if (s != 0) {
        cout << -2 << "\n";
        return;
    }
    for (int x: ans) cout << x << " ";
    cout << "\n";
}
```



## C

推公式，可以发现，题目要求的条件可以转化为`2*s[r] - r - 1 = 2 * s[l] - l`，其中`s[i]`为前缀`[1..i]`的1的数目。

于是可以做一次DP，最后根据DP值回溯即可

```cpp
void solve() {
    int n;
    cin >> n;
    vector <int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    vector <int> s(n+10);
    for (int i = 1; i <= n; i++) {
        s[i] = s[i-1] + a[i-1];
        //cout << s[i] << " " << s[i-1] << " " << a[i] << "\n";
    }
    unordered_map <int, int> m;
    int ans = 0;
    vector <int> f(n+10);
    int mn = 2000;
    for (int i = 1; i <= n; i++) {
        int now = 2 * s[i] - i-1;
        if (a[i-1]==0) continue;
        int q = m[now];
        f[i] = q+1;
        m[now+1] = f[i];
        ans = max(ans, q+1);
    }
    int idx = max_element(begin(f), end(f)) - begin(f);
    vector <int> res;
    res.push_back(idx+1);
    for (int i = idx; i >= 0; i--) {
        if (ans && ans == f[i]) {
            res.push_back(i);
            ans--;
        }
    }
    reverse(all(res));
    cout << res.size() << "\n";
    for (int x: res) cout << x << " ";
    cout << "\n";
}
```

