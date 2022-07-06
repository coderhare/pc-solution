### A
```c++
void solve() {
    ll a, b;
    cin >> a >> b;
    cout << a * b << endl;
}

```
### B
```c++
void solve() {
    ll a, b;
    cin >> a >> b;
    cout << max(0LL, b - a) << endl;
}
```

### C
```c++
void solve() {
    int n;
    cin >> n;
    map <int ,int> cnt;
    for (int i = 0; i < n; i++) {
        int a; cin >> a;
        cnt[a]++;
    }
    bool ok = 1;
    int N = 1e5;
    for (auto && [k, v]: cnt){
        if(v % k != 0){
            ok = 0;
            break;
        }
    }
    cout << (ok?"YES":"NO") << endl;
}
```

### D
贪心，每从反转是将1的序列反转到最后面，如果是一片1可以在一次操作中完成
```c++
void solve() {
    int n;
    cin >> n;
    string s;
    cin >> s;
    s.erase(unique(all(s)), end(s));
    cout << count(all(s), '1') - (s.back() == '1') << endl;
}
```

### E
这个特殊的交换，首先我们需要去考虑交换的距离，接着存下来这些操作的情况，然后求最大公约数，就能满足题意
```c++
void solve() {
    int n;
    cin >> n;
    vector <int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    auto b = a;
    sort(all(b));
    map <int, int> m;
    for (int i = 0; i < n; i++) {
        m[b[i]] = i;
    }
    vector <int> c;
    for (int i = 0; i < n; i++) {
        if(i != m[a[i]]){
            c.push_back(abs(m[a[i]] - i));
        }
    }
    int k = c[0];
    for (int i = 0; i < c.size(); i++) {
        k = gcd(k, c[i]);
    }
    cout << k << endl;
}
```
### F
需要观察一下性质，因为X, Y, Z存在一个至少一个数为奇数，由于A, B, C为质数，这意味着必须会有一个数是偶数，因此其中一个数只能为2.
至此只需要分类讨论即可
```c++
void solve() {
    int x, y;
    cin >> x >> y;
    //A is odd and B is even
    if (x % 2 == 0 && y % 2 == 1) swap(x, y);
    if (x % 2 == 1 and y % 2 == 0){
        cout << 2 << " " << min(x ^ 2, y ^ (x ^ 2)) << " " << max(x ^ 2, y ^ (x ^ 2)) << endl;
        return;
    }
    //A is odd and B is odd
    cout << 2 << " " << min(x ^ 2, y ^ 2) << " " << max(x ^ 2, y ^ 2) << endl;
}
```

### G
蒙的，不太理解为什么可以排序然后分段取最大值。
我一开始觉得是个NP-hard问题，但是数据规模太大了，于是卡了很长时间
```c++
void solve(){
    int n;
    cin >> n;
    vector <int> a(n);
    //if we use mask DP, it will TLE
    ll tot = 0;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
        tot += 1000 - a[i];
    }
    ll b = 0, c = 0;
    sort(all(a));
    ll ans = 0;
    for (int i = 0; i < n; i++) {
        b += a[i];
        tot -= 1000 - a[i];
        chkmax(ans, tot * b);
    }
    b = 0, c = 0;
    tot = accumulate(all(a), 0LL);
    for (int i = 0; i < n; i++) {
        b += 1000 - a[i];
        tot -= a[i];
        chkmax(ans, tot * b);
    }
    cout << ans << endl;
}
```


### F
这种序列操作为了避免后效性问题，一般都是从后面开始操作
```c++
void solve(){
    int n;
    cin >> n;
    string s;
    cin >> s;
    vector <int> id;
    auto chk = [&](char c)->bool{
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    };
    bool rev = 1;
    string ans(n, ' ');
    int l = 0, r = n - 1;
    for (int i = n - 1; i >= 0; i--) {
        if(rev) ans[r--] = s[i];
        else ans[l++] = s[i];
        if(chk(s[i])){
            rev ^= 1;
        }
    }
    cout << ans << endl;
}
```