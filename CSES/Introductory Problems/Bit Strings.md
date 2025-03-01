# Bit Strings
You can view the problem statement and submit [here](https://cses.fi/problemset/task/1617)

## Step 1: Finding the equasion
To find the number of bit strings of size $n$, we need to start from the definition of a bit string (or binary string): a string where each character is either `0` or `1`. This means there are 2 possible values for each character, so there are 2 ways to assign a value to one. 

Now we can use the rule of product, which states that if there are $a$ differentways to do something and $b$ different ways to do something else, there are $a\cdot b$ different ways to do both. This means that there are $\underbrace{2\cdot 2\cdot\dots2}_{\textit{n times}}=2^n$ ways to create a binary string of size $n$.

We can calculate that with a simple for loop:
```cpp
ll ans=1;
for(ll i=0; i<n; i++){
    ans*=2;
}
```

## Step 2: Working with modulo
The value of $2^n$ will grow very fast, resulting in overflow. This is why the problem only asks for that value modulo $10^9+7$. However, because of the overflow, using the modulo operator right before printing the answer. So we need to find a way to keep the answer small at all times.

To do that, we can use a basic property of modular arithmetics: $(a\cdot b)\bmod m=(a\bmod m\cdot b\bmod m)\bmod m$. This means that $(ans\cdot2)\bmod m=((ans\bmod m)\cdot2)\bmod m$, where $m=10^9+7$. In other words, we can replace `ans` with `ans%MOD` after each multiplication, keeping it small enough.

## The code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
const ll MOD=1e9+7  // Instead of typing this value every time we use it, we can now just type `MOD`

int main(){
    ll n;
    cin>>n;

    ll ans=1;
    for(ll i=0; i<n; i++){
        ans*=2;
        ans%=MOD;  // Do this every time to avoid overflow
    }

    cout<<ans<<endl;
}
```

## What we learned from this problem
- **Rule of product** - The fundamental principle of counting, or rule of product, states that if there are $a$ differentways to do something and $b$ different ways to do something else, there are $a\cdot b$ different ways to do both. We can use that in many combinatorics problems, with the fact that there are $2^n$ bit strings of size $n$ being a really common use.
- **Basic modular arithmetics** - As many problems require you to print the answer modulo some value, it's important to have some basic properties of the modulo operation in mind:
  - $(a+b)\bmod m=(a\bmod m+b\bmod m)\bmod m$
  - $(a-b)\bmod m=(a\bmod m-b\bmod m)\bmod m$
  - $(a\cdot b)\bmod m=(a\bmod m\cdot b\bmod m)\bmod m$
  - $(a^b)\bmod m=(a\bmod m)^b\bmod m$
 
  This means that we can use the modulo operator before adding, subtracting, multiplying, or raising it to some power (but **not** dividing).
