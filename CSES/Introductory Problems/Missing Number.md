# Missing Number
You can view the problem statement and submit [here](https://cses.fi/problemset/task/1083)

## Step 1: Keeping track of the values
In competitive programming, we often need a way to easily store and find information about specific value, like in this problem, where we need to know whether a value appears in the input or not. To do that, we can create an array where the $i$-th element isn't the value at position $i$ but a value related to the number $i$. Here, $x_i$ will be equal to $1$ if $i$ appears in the input. The size of $x$ should be the largest possible value plus one, in this case $n+1$, as the numbers will be at most $n$.

## Step 2: Updating the array
Now we must check which values appear in the input array. To do that, we'll initially set all $x_i$ to $0$, then for each element of the input array (say $a_i$) set $x_{a_i}$ to $1$.

```cpp
bool x[n+1]={};  // Create an array with all its elements initially set to 0
for(ll i=0; i<n; i++){
    x[a[i]]=1;  // We found the value a[i]
}
```

## Step 3: Finding the missing number
Finally, we'll just check whether each $x_i$, $1 \leq i \leq n$ is $0$. If it is, then we didn't find $i$ in the input array, so just print it.

```cpp
for(ll i=1; i<=n; i++){
    if(x[i]==0){
        cout<<i<<endl;
    }
}
```

## The code
```cpp
include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main(){
    // Input
    ll n;
    cin>>n;
    ll a[n-1];
    for(ll i=0; i<n-1; i++){
        cin>>a[i];
    }

    bool x[n+1]={};  // Create an array with all its elements initially set to 0
    for(ll i=0; i<n; i++){
        x[a[i]]=1;  // We found the value a[i]
    }

    for(ll i=1; i<=n; i++){
        if(x[i]==0){
            cout<<i<<endl;
        }
    }
```

## What we learned from this problem
- **Storing information about values** - An array can be used to store information about the values of another array, for example whether a value appears in that array. The array with the information about values should have size $n+1$, where $n$ is the maximum value we want to store information about.
