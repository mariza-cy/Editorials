# Inmate
You can view the problem statement and submit [here](https://michanicos.cmscoinformatics.org/#/task/inmate/statement)

> [!NOTE]
> You can skip to any subtask (or the full solution). If you need to read the solution to another subtask to understand the subtask you are currently reading, you will find a note in the beginning of that section.

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
As $m$ is pretty small, we can try all the $2^m$ combinations we can choose to find one that works, or know there are no combinations we can use. We can represent a combination by an integer with $m$ bits, where the $i$-th bit is on if we choose type A of that kind of key and off otherwise. To try all the combinations, we can try all numbers from $0$ to $2^m-1$.

To check the $i$-bit of a variable `k`, we can calculate `k&(1ll<<i)`. If it's greater than zero, that bit is on, otherwise it's off.

```cpp
for(ll k=0; k<(1ll<<m); k++){
    string curr;  // curr: the current combination
    for(ll i=0; i<m; i++){
        if(k&(1ll<<i)) curr+='A';
        else curr+='B';
    }
}
```

### Step 2: Checking a combination
Now we need a way to check if a specific combination can be used to escape. For a combination to be valid, there must be at least one chosen lock in each door, se we can check each door for a chosen lock, setting a boolean `ans` to `false` if we don't find any.

```cpp
bool ans=true;  // ok: true if the current combination is valid
for(ll i=0; i<n; i++){
    bool ok=false;  // curr: true if door i has at least one chosen lock
    for(ll j=0; j<x[i].size(); j++){
        if(curr[x[i][j].second]==x[i][j].first)  // If the key of the current lock's kind is the same type as the lock
            ok=true;
            break;
        }
    }
    if(!ok){  // If we didn't find any chosen keys
        ans=false;
        break;
    }
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

    bool f=false;  // f: true if a valid combination is found
    for(ll k=0; k<(1ll<<m); k++){
        string curr;  // curr: the current combination
        for(ll i=0; i<m; i++){
            if(k&(1ll<<i)) curr+='A';
            else curr+='B';
        }

        bool ans=true;  // ans: true if the current combination is valid
        for(ll i=0; i<n; i++){
            bool ok=false;  // ok: true if door i has at least one chosen lock
            for(ll j=0; j<x[i].size(); j++){
                if(curr[x[i][j].second]==x[i][j].first){  // If the key of the current lock's kind is the same type as the lock
                    ok=true;
                    break;
                }
            }
            if(!ok){  // If we didn't find any chosen keys
                ans=false;
                break;
            }
        }

        if(ans){
            cout<<1<<endl;
            cout<<curr<<endl;
            f=true;
            break;
        }
    }

    if(!f){  // If a valid combination wasn't found
        cout<<0<<endl;
    }
}
