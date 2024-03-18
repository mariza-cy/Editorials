# 1933A - Turtle Puzzle: Rearrange and Negate
You can view the problem statement and submit [here](https://codeforces.com/contest/1933/problem/A)

### Step 1
Let's see what the operations allow us to do. Since we can choose the order of the elements and their final order doesn't affect the result, we can choose an order that helps us choose the optimal subarray. In other words, since the only restriction to which elements we can replace with their opposites is their position, which we can chose without affecting anything else, we can actually choose any elements we want. This means we can choose if we add either $a$<sub>$i$</sub> or $-a$<sub>$i$</sub>.
This means that the maximum sum will occur when we perform the operations in a way that will result in all the negative elements being replaced, so it will be the sum of $|a$<sub>$i$</sub>$|$ among all $i$, $1 \leq i \leq n$.

## Code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main(){
    ll tc;
    cin>>tc;
    while(tc--){
        ll n;
        cin>>n;
        ll a[n], ans=0;
        for(ll i=0; i<n; i++){
            cin>>a[i];
            ans+=abs(a[i]);
        }
        cout<<ans<<endl;
    }

    return 0;
}
```
