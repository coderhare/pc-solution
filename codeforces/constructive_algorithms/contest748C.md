# codeforces contest748 C

> 按tag是constructive problems，但是实际上感觉是greedy的

首先需要明确一点，最少的点依赖于这些走的距离一定是最长的线段，就是说，我不是到处设点，如何判断是否当前点一定是需要设点的呢？需要厘清一下过程：

**当出现往回走的情况的时候，如果说我们走着走着往回走了，这就说明，之前方向的最远处一定是需要走的点，我们走完了往回走寻找下一个点**

```cpp
int main(){
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    //greedy: makes points as least as possible
    //line as long as possible
    string s;
    int n;
    cin >> n >> s;
    unordered_set <int> S;
    int ans = 0;
    for(char & c: s){
        if(c == 'L') c = -1;
        if(c == 'R') c = 1;
        if(c == 'U') c = -2;
        if(c == 'D') c = 2;
    }
    for(int i = 0; i < n; i++){
        if(S.count(s[i])) continue;
        if(S.count(-s[i])) { //当出现了往回走的情况，这说明我们当前深入的点是需要走的，否则不需要如此“大费周章”
            S.clear();
            ans++;
        }
        S.insert(s[i]);
    }
    cout << ans + 1 << endl;
    return 0;
}
```
