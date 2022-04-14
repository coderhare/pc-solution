# codeforces779 div2

### A

> 构造，贪心

```cpp
void solve(){ //要想满足每个＞=2的子串都满足0数目小于1，则最贪心的做法是最多填2个
    int n; string s;
    cin >> n >> s;
    int ans = 0;
    for(int i = 0; i < n; i++){
        if(s[i] == '0'){
            int j = find(begin(s) + i + 1, end(s), '0') - begin(s);
            if(j != n){
                ans += max(0, 3 - (j - i));
                i = j - 1;
            }
        }
    }
    cout << ans << endl;
}
```

### B

> 一眼猜答案，有一些number theory的意味在里面

看样例猜测奇数长度不可行，然后看输出盲猜了一个规律。实际上要想满足，让所有数都%2==0即可。于是奇数在偶数下标，偶数在奇数下标，总共有((n - 2)!)^2种可能。

详细证明：

反证法，假设存在＞2的因子，显然2 < p <= n。我们可以发现数里头被p整除的有⌊n/p⌋个，本身不被p整除的数，可以乘上p的倍数，但只有⌊n/p⌋有这样的优待，所以不满足条件；同理，p==2恰好可以

```cpp
void solve(){
    int n;
    cin >> n;
    ll mod = 998244353;
    if(n & 1) {
        cout << 0 << endl;
        return;
    }
    else{
        //for every digits, we can check if it modulo 2 eq 0
        ll ans = 1;
        for(int i = 2; i <= n; i+=2){
            ans *= (i / 2) * (i / 2);
            ans %= mod;
        }
        cout << ans << endl;
    }
}
```

### C

> constructive algorithm + number theory

考虑每次rotate，最多可能产生影响为c[i + 1] - c[i] <= 1。

显然每次rotate都可能造成上述影响

```cpp
void solve(){
    int n;
    cin >> n;
    vector <int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];
    auto j = find(begin(a), end(a), 1);
    if(j == end(a)){
        cout << "NO\n";
        return;
    }
    rotate(begin(a), j, end(a));
    for(int i = 1; i < n; i++){
        if(a[i] - a[i - 1] > 1 || a[i] == 1){
            cout << "NO\n";
            return;
        }
    }
    cout << "YES\n";
}
```

### D1

由异或的性质，如果说&x之前所有数的某一位为cnt[i]个，那么异或1之后必定不为cnt[i]个，因此，如果说某些位出现次数不为0并且异或后不为cnt[i]，则肯定出现过。

```cpp
void solve(){
    vector <int> cnt(20);
    int l, r;
    cin >> l >> r;
    for(int i = 0; i <= r; i++){
        for(int j = 17; j >= 0; j--){
            if((i >> j & 1)){
                cnt[j]++;
            }
        }
    }
    int a;
    vector <int> cnt1(20);
    for(int i = 0; i <= r; i++){
        cin >> a;
        for(int j = 17; j >= 0; j--){
            if((a >> j & 1) == 1) cnt1[j]++;
        }
    }
    int ans = 0;
    for(int i = 0; i <= 17; i++){
        //cout << cnt1[i] << endl;
        if(cnt1[i] == r + 1){
            ans |= 1 << i;
        }
        else if(cnt1[i] && cnt[i] != cnt1[i]) ans |= 1 << i;
    }
    cout << ans << endl;
}
```
