# Tasks and Deadlines
You can view the problem statement and submit [here](https://cses.fi/problemset/task/1630)

## Step 1: Creating an expression
Let $f_i$ be the time task $i$ is finished and $d_i$ be its deadline. Then we get reward $d_i - f_i$ for each task $i$, so the total reward is:
```math
s = (d_1 - f_1) + (d_2 - f_2) + \dots + (d_n - f_n)
```

## Step 2: Grouping the terms
A useful trick in problems with a large expression is to change it so that similar terms are together. Here, we'll seperate all $d_i$ from all $f_i$:
```math
s = (d_1 - f_1) + (d_2 - f_2) + \dots + (d_n - f_n) = (d_1 + d_2 + \dots + d_n) - (f_1 + f_2 + \dots + f_n)
```

This helps us notice something important: as the sum of deadines can't be changed, the order of the tasks doesn't matter. The only thing that affects the reward is the sum of the finishing times. Therefore, we only need to minimise $x = f_1 + f_2 + \dots f_n$.

## Step 3: Calculating starting times
Now, let $a_i$ be the duration and $s_i$ be the starting time of task $i$, assuming the tasks are sorted in the order they will be completed. We can easily see that the optimal strategy would be to start the first task on time $0$, then each other task immediately after finishing the previous one. So:

```math
s_1 = 0 \implies f_1 = s_1 + a_1 = 0 + a_1 = a_1
```
```math
s_2 = f_1 = a_1 \implies f_2 = s_2 + a_2 = a_1 + a_2
```
```math
s_3 = f_2 = a_1 + a_2 \implies f_3 = s_3 + a_3 = a_1 + a_2 + a_3
```
```math
\dots
```
```math
s_n = f_{n-1} = a_1 + a_2 + \dots + a_{n-1} \implies f_n = s_n + a_n = a_1 + a_2 + \dots + a_n
```

## Step 4: Finding the optimal strategy
We can now reaplace each $f_i$ in the final expression from Step 2:
```math
x = f_1 + f_2 + \dots f_n = (a_1) + (a_1 + a_2) + (a_1 + a_2 + a_3) + \dots + (a_1 + a_2 + \dots + a_n)
```

We can group the terms again, this time using the fact that $a_1$ appears in $n$ clauses, $a_2$ in $n-1$, $a_3$ in $n-2$ and so on, as $a_i$ appears from the $i$-th clause to the last ($n$-th) one:
```math
x = na_1 + (n-1)a_2 + (n-2)a_3 + \dots + 2a_{n-1} + a_n
```

So the earlier we complete a task, the more it affects the total duration, since it also affects all the tasks that are completed afterwards. This means that it's optimal to start with the task with the shortest duration, then the one with the second shortest one, and so on, so that the smallest values of $a_i$ will have the largest impact.

### A small note: Reviewing the observations
Many of the observations we made might seem very simple now, but making them, or being confident that they are true without formal proof can be more challenging than it seems. Sometimes using simple math, like in this problem, is essential to realise which factors actually matter, and which things we need to have in mind. This is the main idea of greedy algorithms, and this problem is a great way to understand how the original problem can be transformed.

## The code
```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main(){
    // Input
    ll n;
    cin>>n;
    ll a[n], d[n];
    for(ll i=0; i<n; i++){
        cin>>a[i]>>d[i];
    }

    sort(a,a+n)  // Sort the durations so we can start from the smallest

    ll x=0;  // x: The sum of finishing times
    for(ll i=0; i<n; i++){
        x+=a[i]*(n-i);
    }

    ll s=0;  // s: The total reward
    for(ll i=0; i<n; i++){
        s+=d[i];  // Add the deadlines
    }
    s-=x;  // Remove the sum of finishing times

    cout<<s<<endl;  // Output
}
```

## What we learned from this problem
- **Grouping** - When we have a large expression, we can try grouping its terms so that similar terms are together. This can help us understand which factors affect the value of the expression, or calculate it more easily.
