# Weird Algorithm
You can view the problem statement and submit [here](https://cses.fi/problemset/task/1068)

## Step 1: Checking the parity
Knowing how to check whether a number $a$ is divisible by another number $b$ is very important in CP. To do that, we can use the modulo operator (%). If $b|a$ ($a$ is divisible by $b$), the remainder of $a$ when divided by $b$ will be $0$. Otherwise, it won't, so we can just check if `a%b==0`. In this case, $a=n$ and $b=2$, since we want to check the parity of $n$:

```cpp
if(n%2==0){
    n=n/2;
}
else{
    n=3*n+1;
}
```

## Step 2: The loop
We must repeat the operations until $n=1$. In other words, we must repeat them for as long as $n\neq1$. To repeat something for as long as a certain condition is met, we can use a while loop:

```cpp
while(n!=1){
    // ...
}
```

## Step 3: A small mistake
If we run a code that prints $n$ and then changes it until it's $1$, we'll notice that it never prints $1$, since it's inside the while loop. To fix it, we can just print $1$ after the loop is finished.

## The code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main(){
    ll n;
    cin>>n;  // Input
    
    while(n!=1){
        cout<<n<<" ";
        if(n%2==0){  // If even
            n=n/2;
        }
        else{  // If odd
            n=3*n+1;
        }
    }
    
    cout<<1<<endl;
}
```

## What we learned from this problem
- **Divisibility** - To check if $a$ is divisible by $b$, we can just check if `a%b==0`.

## Bonus: the Collatz Conjecture
This problem is related to the Collatz Conjecture, a famous unsolved problem in mathematics. 

The original problem is to prove that the value of $n$ will eventually become $1$. The way $n$ changes is unpredictable on the long run, which explains why the problem is still unsolved. However, the algorithm was tested countless times, and the final value is always $1$.

You can find out more [here](https://www.quantamagazine.org/why-mathematicians-still-cant-solve-the-collatz-conjecture-20200922/).
