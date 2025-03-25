# Inmate
You can view the problem statement and submit [here](https://michanicos.cmscoinformatics.org/#/task/inmate/statement)

> [!NOTE]
> You can skip to any subtask (or the full solution). If you need to read the solution to another subtask to understand the subtask you are currently reading, you will find a note in the beggining of that section.

## Subtask 1: Doors only include simple locks (type A)
### Step 1: Finding a strategy
Since doors only include type A locks, no type B locks are needed. Because of this, the inmate is guaranteed to be able to escape if we choose the type A key for all the kinds of locks.

### The code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main(){
    // Input
    ll n, m;  // n: the number of doors, m: the kinds of locks
    cin>>n>>m;
    vector<pair<char,ll>> x[n];  // x[i]: the locks of door i
    for(ll i=0; i<n; i++){
        ll q;
        cin>>q;
        for(ll j=0; j<q; j++){
            bool t;
            ll k;
            cin>>t>>k;
            x[i].push_back({t,k});
        }
    }

    cout<<1<<endl;  // The prisoner can definitely escape
    for(ll i=0; i<m; i++){
         cout<<'A'<<endl;  // We must choose type A for all keys
    }
}
```

## Subtask 2: $Q_i = 1$ for all $1 \leq i \leq N$
### Step 1: Understanding the constraints
All doors have only one lock, so we must choose all the keys that unlock at least one door. We can keep track of all the locks that appear in the input using a boolean array and use them.
```cpp
bool a[m]={}, b[m]={};  // a[i]: true if the key of type A and kind i opens a door, b[i]: the same but with type B keys
for(ll i=0; i<n; i++){
    if(x[i][0].first=='A') a[x[i][0].second]=true;
    else b[x[i][0].second]=true;
}
```

### Step 2: Cases where escaping is impossible
The only problem here is that we can only choose one lock of each kind, so if both locks are needed, the inmate can't escape. We can check that using a boolean `ok`, which is initially set to `true`, that becomes `false` once we find a kind of lock where both types are needed.
```cpp
bool ok=true;  // ok: true if the inmate can escape
for(ll i=0; i<m; i++){
    if(a[i] && b[i]){
        ok=false;
        break;
    }
}
```

### Step 3: Choosing the keys
If only one type of key is needed from each kind, we must choose that one. If neither is needed, we can choose either - here we'll choose type A. We can use a string `ans` to keep track of the keys we've chosen.
```cpp
string ans;  // ans: the keys we will choose
for(ll i=0; i<m; i++){
    if(a[i]) ans+='A';  // If type A is needed
    else if(b[i]) ans+='B';  // If type B is needed
    else ans+='A';  // If no keys of this kind are needed
}
```

### The code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main(){
    // Input
    ll n, m;  // n: the number of doors, m: the kinds of locks
    cin>>n>>m;
    vector<pair<char,ll>> x[n];  // x[i]: the locks of door i
    for(ll i=0; i<n; i++){
        ll q;
        cin>>q;
        for(ll j=0; j<q; j++){
            bool t;
            ll k;
            cin>>t>>k;
            x[i].push_back({t,k});
        }
    }

    bool a[m]={}, b[m]={};  // a[i]: true if the key of type A and kind i opens a door, b[i]: the same but with type B keys
    for(ll i=0; i<n; i++){
        if(x[i][0].first=='A') a[x[i][0].second]=true;
        else b[x[i][0].second]=true;
    }

    bool ok=true;  // ok: true if the inmate can escape
    string ans;  // ans: the keys we will choose
    for(ll i=0; i<m; i++){
        if(a[i] && b[i]){  // If both types are needed
            ok=false;
            break;
        }
        if(a[i]) ans+='A';  // If type A is needed
        else if(b[i]) ans+='B';  // If type B is needed
        else ans+='A';  // If no keys of this kind are needed
    }

    cout<<ok<<endl;
    if(ok) cout<<ans<<endl;
}
```

## Subtask 3: $1 \leq N \leq 100$, $1 \leq M \leq 20$
### Step 1: Brute force
As $m$ is pretty small, we can try all the $2^m$ combinations we can shoose to find one that works, or know there are no combinations we can use. We can represent a combination by an integer with $m$ bits, where the $i$-th bit is on if we choose type A of that kind of key and off otherwise. To try all the combinations, we can try all nubers from $0$ to $2^m-1$.
